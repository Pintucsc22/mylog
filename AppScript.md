## 🏗️ What is Google Apps Script?

**Google Apps Script** is a **JavaScript-based** platform created by Google to **automate tasks** and **extend** Google Workspace apps like:

* **Google Sheets**
* Google Docs
* Google Forms
* Gmail
* Calendar

It's like adding **custom logic** to your Google Sheets — such as buttons, automated reports, email notifications, etc.

---

## 🧠 Key Concepts You Need to Know

Let’s go over the **basics**, one at a time:

---

### 1. ✅ The Script Editor

**Where to write Apps Script code:**

1. Open your **Google Sheet**
2. Go to **Extensions > Apps Script**
3. You'll see a code editor. This is where you write your scripts.

---

### 2. 🔄 Functions: The Building Blocks

Apps Script is organized into **functions** (just like in JavaScript):

```javascript
function myFirstFunction() {
  Logger.log("Hello, world!");
}
```

* `function` is the keyword to define a block of code
* `myFirstFunction` is the name
* Code inside `{}` runs when you call the function

💡 You can run any function manually from the Apps Script editor by selecting it and clicking **▶️ Run**.

---

### 3. 📋 Logging Output

To **see what your code is doing**, use `Logger.log()`:

```javascript
function logExample() {
  Logger.log("This is a message");
}
```

* After running the function, go to **View > Logs** to see what it printed.

---

### 4. 📄 Working with Google Sheets

You can read and write to cells using Apps Script.

#### Example: Write to a Cell

```javascript
function writeToSheet() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  sheet.getRange("A1").setValue("Hello!");
}
```

* `SpreadsheetApp.getActiveSpreadsheet()` → gets the current sheet file
* `.getActiveSheet()` → picks the currently opened sheet tab
* `.getRange("A1")` → selects cell A1
* `.setValue("Hello!")` → writes text to that cell

---

#### Example: Read from a Cell

```javascript
function readFromSheet() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const value = sheet.getRange("A1").getValue();
  Logger.log("Value in A1 is: " + value);
}
```

---

### 5. 📚 Variables

You store data in **variables** using `const` or `let`:

```javascript
const name = "Alex";
let age = 25;
```

Use `const` when the value **won’t change**, and `let` if it **might change**.

---

### 6. 🔁 Loops and Conditions

These are just like JavaScript:

```javascript
function loopExample() {
  for (let i = 1; i <= 5; i++) {
    Logger.log("Count: " + i);
  }
}

function ifExample() {
  const score = 80;
  if (score >= 75) {
    Logger.log("Passed");
  } else {
    Logger.log("Try again");
  }
}
```

---

### 7. ✅ Triggers (Automation!)

Apps Script supports **triggers** to automatically run functions.

There are 2 types:

#### a. **Simple Triggers** (no setup needed):

* `onEdit(e)` → runs every time someone edits the sheet

```javascript
function onEdit(e) {
  Logger.log("Cell edited: " + e.range.getA1Notation());
}
```

#### b. **Installable Triggers** (set up manually):

You can create time-based triggers:

* Run a function every hour
* Run at 11:59 PM every day

👉 Go to **Triggers (⏰ icon)** in the script editor > Add Trigger

---

## 🧰 Summary of Key Objects in Google Sheets

| Object                    | What it does                           |
| ------------------------- | -------------------------------------- |
| `SpreadsheetApp`          | Entry point to access Sheets           |
| `.getActiveSpreadsheet()` | The spreadsheet file you're working on |
| `.getSheetByName(name)`   | Access a specific sheet/tab            |
| `.getRange("A1")`         | Access a specific cell or range        |
| `.setValue()`             | Write data                             |
| `.getValue()`             | Read data                              |
| `.getValues()`            | Read multiple rows/columns (2D array)  |
| `.setValues()`            | Write multiple values (2D array)       |

---

## 🧪 Try This: Your First Real Script

```javascript
function helloWorld() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  sheet.getRange("A1").setValue("Hello, Apps Script!");
}
```

* Paste this into your script editor
* Hit ▶️ to run it
* Look at cell A1 in your Sheet

🎉 That’s your first automation!

---

## 🧮 1. **2D Arrays: Understanding `getValues()`**

### 🧠 What is it?

When you use `.getValues()` in Apps Script, it returns **a 2D array** — an array of **rows**, and each row is an array of **cells**.

```javascript
function readRange() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data = sheet.getRange("A1:B3").getValues();
  Logger.log(data);
}
```

Imagine your sheet has:

| A    | B   |
| ---- | --- |
| Name | Age |
| Alex | 25  |
| Riya | 30  |

The variable `data` will be:

```js
[
  ["Name", "Age"],
  ["Alex", 25],
  ["Riya", 30]
]
```

> 📌 Each row is an array. So `data[1][0]` = `"Alex"`, `data[2][1]` = `30`

---

### ✅ Looping Through a 2D Array

```javascript
function loopData() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data = sheet.getRange("A2:B4").getValues();

  for (let i = 0; i < data.length; i++) {
    const row = data[i];
    Logger.log("Row " + (i+1) + ": Name=" + row[0] + ", Age=" + row[1]);
  }
}
```

---

## 🗓️ 2. **Working with Dates**

### a. 📅 Get Current Date

```javascript
const today = new Date();
```

### b. 🕓 Set time to 00:00:00 (for clean comparisons)

```javascript
today.setHours(0, 0, 0, 0);
```

### c. ✅ Compare Two Dates (Same Day?)

```javascript
function isSameDay(d1, d2) {
  return d1.getFullYear() === d2.getFullYear() &&
         d1.getMonth() === d2.getMonth() &&
         d1.getDate() === d2.getDate();
}
```

### d. 🧪 Timestamp a cell

```javascript
sheet.getRange("C2").setValue(new Date());
```

> Google Sheets stores dates as numbers, so always format the cell correctly.

---

## 🧰 3. **Creating Custom Menus & Buttons**

### a. 📜 Add a menu to Google Sheets

```javascript
function onOpen() {
  const ui = SpreadsheetApp.getUi();
  ui.createMenu("🚀 My Scripts")
    .addItem("Say Hello", "sayHello")
    .addItem("Run Task", "startTask")
    .addToUi();
}

function sayHello() {
  SpreadsheetApp.getUi().alert("Hello from Apps Script!");
}
```

* `onOpen()` runs every time the sheet is opened
* The menu appears in the menu bar

### b. 🧷 Add a Button (Image or Drawing)

1. Insert > Drawing
2. Add a button shape or image
3. Assign Script: type the name of a function like `sayHello`

---

## 🚨 4. **Error Handling (try...catch)**

Sometimes things break (e.g., bad data, missing sheet). Use `try...catch` to avoid script crashes.

```javascript
function safeCode() {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("MissingSheet");
    if (!sheet) throw new Error("Sheet not found!");

    sheet.getRange("A1").setValue("Hello!");
  } catch (error) {
    Logger.log("❌ Error: " + error.message);
    SpreadsheetApp.getUi().alert("Something went wrong: " + error.message);
  }
}
```

> ✅ Always handle expected errors gracefully!

---

## 🔌 5. **Connecting Sheets, Forms, and Gmail**

### a. ✅ Send Email from a Sheet

```javascript
function sendEmailExample() {
  const email = "someone@example.com";
  const subject = "Hello from Apps Script";
  const message = "This is a test email.";
  MailApp.sendEmail(email, subject, message);
}
```

> 💬 Use this to send reminders or reports automatically!

---

### b. 📋 Connect Google Form to Sheet

When a form is submitted, you can run code using the trigger `onFormSubmit(e)`

```javascript
function onFormSubmit(e) {
  const responses = e.values; // array of submitted values
  Logger.log("Form submitted: " + responses.join(", "));
}
```

Set up this function as a trigger:

* In Apps Script: **Triggers > + Add Trigger**
* Choose: `onFormSubmit`, from spreadsheet or form

---

### c. 🔗 Copy Data from One Sheet to Another

```javascript
function copyData() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const source = ss.getSheetByName("Sheet1");
  const target = ss.getSheetByName("Sheet2");

  const data = source.getRange("A2:B10").getValues();
  target.getRange("A2:B10").setValues(data);
}
```

---