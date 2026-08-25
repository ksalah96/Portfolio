# Methods and Activities for Input and Output

A UiPath RPA workflow that automates data entry into the **DoubleUI** desktop application, reading transaction inputs from an Excel workbook and writing back the resulting transaction numbers.

## 🎯 Overview

This project demonstrates the use of core UiPath input/output activities (`Type Into`, `Click`, `Get Text`) combined with Excel automation to drive a simple desktop app end-to-end:

1. Read a table of transactions from an Excel workbook.
2. For each row, enter the **Cash In** and **On Us Check** values into the DoubleUI application.
3. Click **Accept** to submit the transaction.
4. Capture the resulting **Transaction Number** displayed by the app.
5. Write the transaction number back into the Excel workbook.

## ⚙️ How It Works

The main workflow (`Main.xaml`) is structured as follows:

- **Read Range** – Reads all rows from `DoubleUI-Transactions.xlsx` (Sheet1) into a DataTable.
- **For Each Row** – Iterates over every transaction record:
  - **Type Into "Cash In"** – Enters the `CashIn` value from the current row.
  - **Type Into "On Us Check"** – Enters the `OnUsCheck` value from the current row.
  - **Click "Accept"** – Submits the transaction in the DoubleUI app.
  - **Get Text** – Reads the generated **Transaction Number** from the app.
  - **Write Cell** – Writes the transaction number back to column `C` of the workbook, on the corresponding row.

The automation targets the DoubleUI app window using UI Automation selectors and fuzzy targeting for resilience against minor UI changes.

## 📁 Project Structure

```
Methods and Activities for Input and Output/
├── Main.xaml                    # Main workflow
├── DoubleUI.exe                 # Sample target application
├── DoubleUI-Transactions.xlsx   # Input/output data file (Cash In, On Us Check, Transaction Number)
├── project.json                 # UiPath project configuration
├── project.uiproj                # UiPath project file
├── entry-points.json            # Workflow entry points
├── .screenshots/                # UI targeting screenshots
└── .objects, .entities, .settings, .storage, .templates, .tmh  # UiPath Studio system folders
```

## 📋 Requirements

- **UiPath Studio** (compatible with modern Application Card / UI Automation Next activities)
- **UiPath.Excel.Activities**
- **UiPath.UIAutomationNext.Activities**
- Microsoft Excel (or a compatible engine) to open `DoubleUI-Transactions.xlsx`

## ▶️ How to Run

1. Open the project in **UiPath Studio** via `project.json`.
2. Ensure `DoubleUI.exe` and `DoubleUI-Transactions.xlsx` are present in the project folder.
3. Populate `DoubleUI-Transactions.xlsx` with `CashIn` and `OnUsCheck` values in the input columns.
4. Run `Main.xaml`.
5. The **DoubleUI** application will launch automatically, and each row will be processed sequentially, with transaction numbers written back to the workbook.

## 🧠 Key Learning Points

- Using **Application Card** to manage attach/launch/close behavior for a target desktop app.
- Iterating Excel data with **For Each Row** and DataTable variables.
- Reliable UI targeting using fuzzy selectors and anchors.
- Reading and writing data back to Excel as part of an automated loop (Read Range / Write Cell).

---

**Author:** Khaled Salah
