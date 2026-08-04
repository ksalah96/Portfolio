# RPA Challenge

A UiPath automation project built to solve the classic [RPA Challenge](https://www.rpachallenge.com/) — a well-known exercise used to test data-driven web automation skills.

## 📋 Overview

The bot reads applicant data from an Excel file and automatically fills out the web form on rpachallenge.com for every row in the dataset, submitting each entry in sequence.

**Workflow logic (`Main.xaml`):**
1. Reads all rows from `challenge.xlsx` (sheet `data`) into a DataTable.
2. Opens the RPA Challenge site in Chrome and clicks **Start**.
3. Loops through each row of the DataTable and, for every row:
   - Types the **First Name**, **Last Name**, **Company Name**, **Role in Company**, **Address**, **Email**, and **Phone Number** into their respective form fields.
   - Clicks **Submit**.
4. Repeats until all rows have been processed.

## 🗂 Project Structure

```
RPA-Challenge/
├── Main.xaml          # Main automation workflow
├── challenge.xlsx      # Input data (applicant records)
└── README.md
```

## 📊 Input Data

`challenge.xlsx` contains a `data` sheet with the following columns:

| Column           | Description                  |
|------------------|-------------------------------|
| First Name       | Applicant's first name        |
| Last Name        | Applicant's last name         |
| Company Name     | Company the applicant belongs to |
| Role in Company  | Job title / role              |
| Address          | Street address                |
| Email            | Email address                 |
| Phone Number     | Contact number                |

## 🛠 Requirements

- [UiPath Studio](https://www.uipath.com/product/studio) (Windows)
- UiPath.Excel.Activities and UiPath.UIAutomationNext (installed automatically via project dependencies)
- Google Chrome with the UiPath extension enabled
- Internet access (the bot navigates to `rpachallenge.com`)

## ▶️ How to Run

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/RPA-Challenge.git
   ```
2. Open the project folder in **UiPath Studio**.
3. Make sure `challenge.xlsx` is present in the project's root directory (the workflow references it as a relative path).
4. Run `Main.xaml` (F5 or **Run File**).
5. The bot will open the challenge site, click Start, and begin filling/submitting the form for each row in the spreadsheet.

## ⚠️ Notes

- The workflow uses UI selectors targeting the form fields by their labels (e.g. `First Name`, `Company Name`). The RPA Challenge site shuffles field positions between rounds — if selectors stop matching after an update to the site, they may need to be re-recorded in UiPath Studio.
- Timing/selector reliability can vary depending on connection speed; adjusting timeouts in the `TypeInto` activities can help if the bot runs into element-not-found errors.

## 📄 License

Feel free to use or adapt this project for learning purposes.
