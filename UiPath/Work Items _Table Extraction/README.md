# ACME System 1 - Work Items Table Extraction

An automated Robotic Process Automation (RPA) solution built with **UiPath** designed to log into ACME System 1, navigate to the Work Items module, extract structured tabular data across all paginated pages, and export the extracted records directly into an Excel workbook.

---

## 📌 Project Overview

This UiPath automation automates the end-to-end process of extracting work items from the **ACME System 1** web application:
1. **Authentication**: Securely requests user credentials and logs into the ACME System 1 platform.
2. **Navigation**: Navigates from the dashboard to the Work Items table.
3. **Data Extraction**: Extracts all work item records spanning multiple pages via structured web scraping / Modern UI Table Extraction.
4. **Excel Export**: Stores and writes the extracted tabular data into `Work Items.xlsx`.

---

## 🛠️ Architecture & Workflows

The solution follows a modular workflow architecture:

| Workflow File | Description |
| :--- | :--- |
| `Main.xaml` | Master controller orchestrating the login, navigation, data extraction, and Excel export sequences. |
| `Login to ACME Website.xaml` | Prompts for user credentials via `Request Credential` and authenticates into ACME System 1 (`https://acme-test.uipath.com/login`). |
| `Navigates to the Work Items page.xaml` | Accesses the ACME Dashboard and clicks through to open the Work Items page. |
| `Extracts the data.xaml` | Scrapes paginated data table items (`WIID`, `Description`, `Type`, `Status`, `Date`) from `https://acme-test.uipath.com/work-items`. |

---

## 📊 Extracted Data Schema

The extracted Excel file (`Work Items.xlsx`) contains the following columns:
- **WIID**: Work Item ID
- **Description**: Summary / Description of the work item
- **Type**: Work Item Type (e.g., WI1, WI2, WI3, WI4, WI5)
- **Status**: Current status (e.g., Open, Completed)
- **Date**: Creation / Log date

---

## 🚀 Prerequisites & Setup

1. **UiPath Studio** (Modern Experience enabled).
2. **Google Chrome** with the **UiPath Web Automation Extension** installed and enabled.
3. An active account on [ACME System 1](https://acme-test.uipath.com/).

---

## ⚙️ How to Run

1. Clone or download the repository to your local machine:
   ```bash
   git clone https://github.com/<your-username>/Work-Items-Table-Extraction.git
   ```
2. Open the project via `project.json` in **UiPath Studio**.
3. Click **Run File** on `Main.xaml` or press `F5`.
4. Enter your ACME System 1 email and password when prompted.
5. Once completed, verify the generated `Work Items.xlsx` file in the project root directory.

---

## 👤 Author

**Khaled Salah**
