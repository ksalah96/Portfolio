# ACME System 1 - Automated Invoice Details Extractor

An automated Robotic Process Automation (RPA) solution built with **UiPath Studio** using the **Modern Design Experience**. This automation processes invoice search queries from an Excel file, logs into the ACME System 1 web portal, retrieves detailed invoice records, and updates the spreadsheet.

---

## 📌 Project Overview

The objective of this automation is to eliminate manual data entry and lookup by automating the end-to-end extraction of invoice data:
1. Reads input invoice records from an Excel workbook (`VendorID_Input.xlsx`).
2. Automates browser login to the **ACME System 1** enterprise portal.
3. Navigates to the **Search for Invoice** interface.
4. Iterates through each invoice number to search and extract:
   - **Vendor Tax ID**
   - **Invoice Item / Description**
   - **Total Amount**
   - **Invoice Date**
5. Updates the Excel data table with the extracted details in real time and saves the final output.

---

## 🚀 Workflow Architecture

The project is modularized into reusable XAML workflows:

| Workflow File | Description |
| :--- | :--- |
| `Main.xaml` | Orchestrates the entire process: reads the Excel input, triggers login/navigation, loops through records, calls extraction modules, and writes results back to Excel. |
| `Login To Acme Website.xaml` | Navigates to ACME System 1 login page and logs in using modern UI Automation activities and credential inputs. |
| `Navigate To Search Invoice.xaml` | Navigates from the dashboard menu (*Invoices* → *Search for Invoice*). |
| `Search For Current Invoice.xaml` | Enters the specific `Invoice Number` into the search box and triggers the search. |
| `Extract Invoice Details.xaml` | Scrapes target data fields (`Vendor Tax ID`, `Invoice Item`, `Total`, `Date`) from the search results table. |
| `Navigate Back To Invoice Search.xaml` | Clicks navigation links to return to the invoice search page for the next iteration. |

---

## 🛠️ Tech Stack & Dependencies

- **Platform:** UiPath Studio (Windows / Modern Experience)
- **Language:** VB.NET / XAML
- **Packages Used:**
  - `UiPath.UIAutomationNext.Activities` (Modern App/Web UI Automation)
  - `UiPath.System.Activities`
  - `UiPath.Excel.Activities`

---

## ⚙️ Prerequisites & Setup

1. **Prerequisites:**
   - UiPath Studio installed.
   - Google Chrome with the **UiPath Web Automation** extension enabled.
   - An active account on [ACME System 1](https://acme-test.uipath.com/).

2. **Configuration:**
   - Open `Login To Acme Website.xaml` and configure your ACME credentials (Email & Password).
   - Ensure `VendorID_Input.xlsx` is placed in the project root directory with the appropriate input column headers (`Invoice Number`, `Vendor Tax ID`, `Invoice Item`, `Total`, `Date`).

3. **Running the Automation:**
   - Open the project in UiPath Studio.
   - Run `Main.xaml` or execute via UiPath Assistant.

---

## 👤 Author

**Khaled Salah**
