
/** ───────────────────────────────
 * 📅 DATE UTILITIES
 *───────────────────────────────*/
function isSameDay(d1, d2) {
  return d1 instanceof Date && d2 instanceof Date &&
         d1.getFullYear() === d2.getFullYear() &&
         d1.getMonth() === d2.getMonth() &&
         d1.getDate() === d2.getDate();
}

function getToday() {
  const d = new Date();
  d.setHours(0, 0, 0, 0);
  return d;
}

/** ───────────────────────────────
 * 📋 SHEET HELPERS
 *───────────────────────────────*/
function getSheet(name) {
  return SpreadsheetApp.getActiveSpreadsheet().getSheetByName(name);
}

function getSheetData(sheet, range = "A8:AF1000") {
  return sheet.getRange(range).getValues();
}

function isAHEmptyRow(row) {
  return row.slice(0,8).every(v => v === "" || v === null);
}

function findFirstEmptyInColumn(values, colIndex = 0) {
  for (let i = 0; i < values.length; i++) {
    if (!values[i][colIndex]) return i;
  }
  return -1;
}

/** ───────────────────────────────
 * ▶️ START TODAY
 *───────────────────────────────*/
function startToday() {
  const sheet = SpreadsheetApp.getActiveSheet();
  if (!sheet || sheet.getName() !== "ProjectSheet") return;
  const data = getSheetData(sheet);
  const today = getToday();

  let todayRow = data.findIndex(row => isSameDay(row[0], today));

  if (todayRow === -1) {
    const emptyRow = findFirstEmptyInColumn(data, 0); // column A
    if (emptyRow !== -1) {
      sheet.getRange(8 + emptyRow, 1).setValue(today); // Add date
      todayRow = emptyRow;
    }
  }

  // Add timestamp in AA if missing
  data.forEach((row, i) => {
    if (isSameDay(row[0], today) && !row[26]) {
      sheet.getRange(8 + i, 27).setValue(new Date()); // Column AA
    }
  });
  sheet.getRange("E4").setValue(false);
}


/** ───────────────────────────────
 * 📥 IMPORT DROP LIST
 *───────────────────────────────*/
function importDropList(sheet) {
  if (!sheet) sheet = getSheet("ProjectSheet");
  const dropSheet = getSheet("Drop List");
  if (!sheet || !dropSheet) return;

  const dropData = dropSheet.getRange("A16:D25").getValues();
  if (!dropData || dropData.length === 0) {
    Logger.log("❌ Drop List is empty.");
    return;
  }

  const fullData = sheet.getRange("A8:H1000").getValues();

  Logger.log(`🔹 Found ${dropData.length} drop rows`);

  dropData.forEach((dropRow, index) => {
    if (!dropRow.some(v => v !== "" && v !== null)) {
      Logger.log(`⏭ Drop List row ${16 + index} is blank, skipping`);
      return;
    }

    // Check if same task already exists in column A (duplication check)
    const existsInA = fullData.some(row => row[0] && row[0] === dropRow[0]);
    if (existsInA) {
      Logger.log(`⏭ Drop List row ${16 + index} already exists in ProjectSheet column A, skipping`);
      return;
    }

    // Find the first empty row in column A where C–F are empty
    let targetRow = -1;
    for (let i = 0; i < fullData.length; i++) {
      if (!fullData[i][0] && fullData[i].slice(2, 6).every(v => v === "" || v === null)) {
        targetRow = 8 + i;
        break;
      }
    }

    if (targetRow === -1) {
      Logger.log(`❌ No suitable empty row found in ProjectSheet for drop row ${16 + index}`);
      return;
    }

    // Insert drop row into C–F
    sheet.getRange(targetRow, 3, 1, 4).setValues([dropRow]); // C-F
    //sheet.getRange(targetRow, 9).setValue("Pending"); // I
    sheet.getRange(targetRow, 35).clearContent(); // AI helper

    Logger.log(`✅ Drop List row ${16 + index} imported into ProjectSheet row ${targetRow}`);

    // Update local copy
    fullData[targetRow - 8].splice(2, 4, ...dropRow); // C-F
  });

  Logger.log("🔹 Drop List import complete");
}


/** ───────────────────────────────
 * 📤 SUBMIT TODAY (Manual)
 *───────────────────────────────*/
function submitToday() {
  const sheet = SpreadsheetApp.getActiveSheet();
  const ui = SpreadsheetApp.getUi();
  const today = getToday();
  const dataRange = sheet.getRange("A8:AI1000");
  const data = dataRange.getValues();
  const submitted = [];
  const issues = [];

  // Step 0️⃣: Skip if any today's row already has AB (col 28)
  const alreadySubmitted = data.some(row => isSameDay(row[0], today) && row[27]);
  if (alreadySubmitted) {
    ui.alert("⚠️ Submission skipped.\nToday's rows already have timestamp in AB (column 28).");
    Logger.log("⚠️ Submission skipped.");
    return;
  }

  // Step 1️⃣: Validate B–F and J
  const incompleteRows = [];
  for (let i = 0; i < data.length; i++) {
    const row = data[i];
    if (!isSameDay(row[0], today)) continue;
    const bToF = row.slice(1, 6);
    const statusJ = String(row[9] || "").trim();
    if (bToF.some(v => v === "" || v === null) || statusJ === "")
      incompleteRows.push(i + 8);
  }

  if (incompleteRows.length > 0) {
    ui.alert("⚠️ Submission cancelled.\nEmpty B–F or J in rows:\n" + incompleteRows.join(", "));
    Logger.log("Cancelled. Empty in rows: " + incompleteRows.join(", "));
    return;
  }

  // Step 2️⃣: Batch update timestamps in AB (col 28)
  const now = new Date();
  const abUpdates = data.map(row => (isSameDay(row[0], today) && !row[27]) ? [now] : [row[27] || ""]);
  sheet.getRange(8, 28, abUpdates.length, 1).setValues(abUpdates);
  sheet.getRange(8, 28, abUpdates.length, 1).setNumberFormat("HH:mm:ss");

  // Step 3️⃣: Carry-forward logic (batch)
  const fullAH = sheet.getRange("A8:H1000").getValues();
  const carryForwardWrites = [];

  for (let i = 0; i < data.length; i++) {
    const row = data[i];
    const rowNum = i + 8;
    if (!isSameDay(row[0], today)) continue;

    const status = String(row[9] || "").trim();
    const carryFlag = row[34];
    const allowed = ["Completed", "Not Completed", "Project Cancelled", "Pending"];
    if (!allowed.includes(status)) {
      issues.push(`Row ${rowNum}: Invalid status '${status}'`);
      continue;
    }

    if ((status === "Pending" || status === "Not Completed") && row[3] && !carryFlag) {
      const emptyIndex = fullAH.findIndex(r => r.slice(0, 8).every(v => !v));
      if (emptyIndex !== -1) {
        const targetRow = 8 + emptyIndex;
        carryForwardWrites.push({
          targetRow,
          cfValues: [row.slice(2, 6), status, "Carried Forward", new Date(), rowNum]
        });
        // mark as filled in memory to avoid duplicate empty row use
        fullAH[emptyIndex][0] = "FILLED";
      } else {
        issues.push(`Row ${rowNum}: No empty row found for carry forward`);
      }
    }
    submitted.push(rowNum);
  }

  // 🔄 Batch apply carry-forward writes
  carryForwardWrites.forEach(w => {
    const { targetRow, cfValues } = w;
    sheet.getRange(targetRow, 3, 1, 4).setValues([cfValues[0]]); // C–F
    sheet.getRange(targetRow, 32).setValue(cfValues[1]); // AF
    sheet.getRange(cfValues[4], 33).setValue(cfValues[2]); // AG
    sheet.getRange(cfValues[4], 35).setValue(cfValues[3]); // AI
  });

  // Step 4️⃣: Summary
  showSummary(ui, submitted, issues);

  // Step 5️⃣: Import Drop List
  importDropList(sheet);
}




/** ───────────────────────────────
 * 🕒 AUTO SUBMIT (Night)
 *───────────────────────────────*/
function autoSubmitAtNight() {
  const sheet = getSheet("ProjectSheet");
  if (!sheet) return;

  const today = getToday();
  let data = getSheetData(sheet);

  // Step 1️⃣: Fill defaults for B–F and J
  const updatesB = [], updatesC = [], updatesD = [], updatesE = [], updatesF = [], updatesJ = [];
  let anyTodayRow = false;

  data.forEach((row, i) => {
    if (isSameDay(row[0], today)) {
      anyTodayRow = true;
      updatesB.push([row[1] || "Accepted"]);
      updatesC.push([row[2] || "Others"]);
      updatesD.push([row[3] || "No"]);
      updatesE.push([row[4] || "Low"]);
      updatesF.push([row[5] || "0"]);

      let status = row[9]; // J column
      if (!status) status = "Not Completed";
      if (status === "Planning" || status === "Running") status = "Pending";
      updatesJ.push([status]);
    } else {
      updatesB.push([row[1]]);
      updatesC.push([row[2]]);
      updatesD.push([row[3]]);
      updatesE.push([row[4]]);
      updatesF.push([row[5]]);
      updatesJ.push([row[9]]);
    }
  });

  // Step 2️⃣: Write all updates in batch
  const lastRow = data.length + 7;
  sheet.getRange("B8:B" + lastRow).setValues(updatesB);
  sheet.getRange("C8:C" + lastRow).setValues(updatesC);
  sheet.getRange("D8:D" + lastRow).setValues(updatesD);
  sheet.getRange("E8:E" + lastRow).setValues(updatesE);
  sheet.getRange("F8:F" + lastRow).setValues(updatesF);
  sheet.getRange("J8:J" + lastRow).setValues(updatesJ);

  if (!anyTodayRow) {
    Logger.log("No rows for today found, skipping auto-submit.");
    return;
  }

  // Step 3️⃣: Re-fetch data after defaults
  data = getSheetData(sheet);

  // Step 4️⃣: Verify all today rows A–F and J are filled
  const incompleteRows = [];
  data.forEach((row, i) => {
    if (isSameDay(row[0], today)) {
      const checkCells = [row[0], row[1], row[2], row[3], row[4], row[5], row[9]]; // A-F + J
      if (checkCells.some(v => v === "" || v === null)) {
        incompleteRows.push(i + 8);
      }
    }
  });

  if (incompleteRows.length > 0) {
    Logger.log("⚠️ Auto-submit cancelled, incomplete today rows: " + incompleteRows.join(", "));
    return;
  }

  // Step 5️⃣: Call submitToday()
  Logger.log("✅ Auto-submit: all today rows ready, calling submitToday()");
  submitToday();
}




/** ───────────────────────────────
 * 🖊️ ON EDIT
 *───────────────────────────────*/
function onEdit(e) {
  const sheet = e.range.getSheet();
  if (!sheet || sheet.getName() !== "ProjectSheet") return;

  const row = e.range.getRow();
  const col = e.range.getColumn();
  const val = e.value ? e.value.toString().trim() : "";

  // Button triggers
  if (e.range.getA1Notation() === "D4" && val === "TRUE") {
    startToday();
    e.range.setValue(false);
    return;
  }

  if (e.range.getA1Notation() === "E4" && val === "TRUE") {
    submitToday();
    e.range.setValue(false);
    return;
  }

  // Timestamp logic
  if (row >= 8) {
    if (col === 2 && val === "Accepted") {
      const cell = sheet.getRange(row, 29); // Column AC
      if (!cell.getValue()) cell.setValue(new Date());
    }

    if (col === 10) { // Column J
      if (val === "Running") {
        const cell = sheet.getRange(row, 30); // AD
        if (!cell.getValue()) cell.setValue(new Date());
      } else if (val === "Completed") {
        const cell = sheet.getRange(row, 31); // AE
        if (!cell.getValue()) cell.setValue(new Date());
      } else if (val === "Completed_Waiting") {
        const cell = sheet.getRange(row, 36); // AJ
        if (!cell.getValue()) cell.setValue(new Date());
      }
    }
  }
}


/** ───────────────────────────────
 * 🧾 SHOW SUMMARY
 *───────────────────────────────*/
function showSummary(ui, submittedRows, issues) {
  const msg = `✅ Submitted rows: ${submittedRows.join(", ")}` +
              (issues.length ? `\n⚠️ Issues:\n${issues.join("\n")}` : "");
  if (ui) ui.alert(msg); else Logger.log(msg);
}


