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

