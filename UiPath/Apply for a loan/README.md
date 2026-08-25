# UiBank - Automated Loan Application

An automated Robotic Process Automation (RPA) workflow built with **UiPath Studio** using modern UI automation and the **App/Web Recorder**. The bot automates the end-to-end process of submitting a loan application on the **UiBank** web portal and extracting the resulting Loan ID.

---

## 📌 Project Overview

This project streamlines the loan application submission on the [UiBank](https://uibank.uipath.com/loans) web platform. The bot launches the browser, navigates through the loan application form, populates applicant details, submits the request, and retrieves the generated Loan ID for confirmation.

---

## 🚀 Features & Workflow Steps

1. **Browser Navigation:** Opens Google Chrome and navigates to the UiBank Loan Portal (`https://uibank.uipath.com/loans`).
2. **Initiate Loan Application:** Clicks the **Apply For A Loan** button.
3. **Form Filling:**
   - **Email Address:** Inputs the applicant's email.
   - **Loan Amount:** Enters requested loan amount.
   - **Loan Term:** Selects loan tenure/term from dropdown.
   - **Yearly Income:** Fills annual income before taxes.
   - **Age:** Enters the applicant's age.
4. **Application Submission:** Clicks **Submit Loan Application**.
5. **Data Extraction & Output:** Extracts the unique generated **Loan ID** from the confirmation page and displays it in a Message Box.

---

## 🛠️ Built With & Prerequisites

- **UiPath Studio** (Windows / Modern Design Experience)
- **Activity Packages:**
  - `UiPath.UIAutomationNext.Activities`
  - `UiPath.System.Activities`
- **Browser:** Google Chrome (with UiPath Web Automation extension enabled)
- **Target Portal:** [UiBank Loan Application](https://uibank.uipath.com/loans)

---

## 📂 Project Structure

```text
Apply for a loan/
├── .entities/
├── .objects/
├── .project/
├── .screenshots/
├── .settings/
├── .templates/
├── .tmh/
├── entry-points.json
├── Main.xaml          # Main automation workflow
├── project.json       # Project dependencies and configuration
└── README.md          # Project documentation
```

---

## ⚙️ How to Run

1. Clone or download this repository:
   ```bash
   git clone https://github.com/<your-username>/Apply-for-a-loan.git
   ```
2. Open `project.json` or `Main.xaml` in **UiPath Studio**.
3. Ensure the **UiPath extension for Chrome** is installed and enabled.
4. Click **Run** or press `F5` in UiPath Studio.

---

## 👤 Author

**Author:** Khaled Salah
