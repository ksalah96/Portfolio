# 🦄 Unicorn Name Generator Automation

An end-to-end cloud automation built with **UiPath Studio Web** that integrates Google Sheets, web UI automation, Google Drive, and Gmail to dynamically generate and distribute personalized unicorn names.

## 📋 Overview

This project automates the process of reading user details from a Google Sheet, querying the [RPA Samples Unicorn Name Generator](https://www.rpasamples.com/unicornname) web application, updating the spreadsheet with the results, and emailing the completed workbook as an Excel attachment.

## ⚙️ How It Works

1. **Manual Trigger:** Starts the cloud automation on demand.
2. **Browser Initialization:** Launches Google Chrome and navigates to `https://www.rpasamples.com/unicornname`.
3. **Data Iteration (Google Sheets):** Connects to the `Unicorn_Name_Iterations` spreadsheet (`Sheet1`) and loops through each row containing a **Name** and **Birthday Month**.
4. **Web UI Automation:**
   * Enters the person's name into the text field.
   * Selects the corresponding birthday month from the dropdown menu.
   * Clicks the **Get Name** button.
   * Extracts the generated unicorn name from the results panel.
5. **Data Update:** Writes the generated unicorn name back into Column C of the active row in the Google Sheet.
6. **File Export (Google Drive):** Downloads the updated `Unicorn_Name_Iterations` Google Sheet as a `.xlsx` file.
7. **Email Notification (Gmail):** Creates an email draft addressed to `automation-first@uipath.com` with the subject *"Unicorn Name Exercise"* and attaches the exported `.xlsx` workbook.

## 🛠️ Technologies & Integrations Used

* **Platform:** UiPath Studio Web
* **UI Automation:** `UiPath.UIAutomationNext.Activities` (Chrome Browser Automation)
* **Google Workspace Integration (`UiPath.GSuite.Activities`):**
  * **Google Sheets:** *For Each Row in Spreadsheet*, *Write Cell*
  * **Google Drive:** *Download File*
  * **Gmail:** *Send Email* (configured to save as draft)

## 📂 Prerequisites & Setup

To run or replicate this project in your own UiPath Studio Web environment:

1. **UiPath Cloud Account:** Access to UiPath Automation Cloud and Studio Web.
2. **Integration Service Connections:** Configure active connections in UiPath Integration Service for:
   * Google Sheets
   * Google Drive
   * Gmail
3. **Input Spreadsheet:** Create a Google Sheet named `Unicorn_Name_Iterations` with `Sheet1` containing the following column headers in Row 1:
   * Column A: `Name`
   * Column B: `Birthday Month`
   * Column C: `Unicorn Name`

## 🚀 Usage

1. Import the project workflow (`Main.xaml`) into **UiPath Studio Web**.
2. Re-link your personal Google Workspace connections (Sheets, Drive, Gmail) in the respective activity cards.
3. Populate your `Unicorn_Name_Iterations` Google Sheet with names and birth months.
4. Click **Run** to execute the automation.

---

**Author:** Khaled Salah