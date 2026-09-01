# Generate Employee ID and Mail ID

A UiPath RPA workflow that automates HR employee onboarding by collecting new-hire details, submitting them through a web-based onboarding form, and retrieving the auto-generated Employee ID and company email address.

## Overview

This project demonstrates an end-to-end UiPath automation built with **modern UI Automation activities** (App/Web Recorder cards). It simulates a simple HR onboarding process:

1. The robot prompts the user for new employee details via Input Dialogs.
2. It opens the bundled `HR_Onboarding_Form.html` form in Chrome.
3. It fills in and submits the form.
4. It reads back the system-generated **Employee ID** and **Company Email** from the form's results panel.
5. It displays the final credentials in a Message Box.

## Project Structure

| File | Description |
|---|---|
| `Main.xaml` | Main UiPath workflow containing the full automation logic |
| `HR_Onboarding_Form.html` | Standalone HTML form used as the target application (simulates an HR onboarding portal) |

## How It Works

### 1. Data Collection (Input Dialogs)
The robot asks for the following fields one at a time:
- First Name
- Last Name
- Title / Role
- Date of Birth
- Mobile Number
- Gender

### 2. Form Automation (Chrome)
Using a `NApplicationCard` (App/Web session), the workflow:
- Opens `HR_Onboarding_Form.html` in Chrome
- Types the collected values into the corresponding fields (`First Name`, `Last Name`, `Title / Role`, `Date of Birth`, `Mobile Number`)
- Selects the `Gender` dropdown
- Checks the "I accept the terms and conditions" checkbox
- Clicks **Submit & Generate Credentials**

### 3. Data Extraction
The HTML form's embedded JavaScript auto-generates:
- **Employee ID** — format `EMP-YYYY-####` (year + random 4-digit number)
- **Company Email** — format `firstname.lastname@company.com`

The robot uses `Get Text` activities to capture these values from the results card into the `employee_id` and `employee_email` variables.

### 4. Output
A final **Message Box** displays:
```
Employee ID: EMP-2026-XXXX
Generated Email: firstname.lastname@company.com
```

## Requirements

- UiPath Studio / Studio Web (modern UI Automation activity package)
- Google Chrome (with the UiPath Extension for Chrome installed)
- `UiPath.UIAutomationNext.Activities` package

## Setup & Run

1. Clone this repository.
2. Open the project in UiPath Studio.
3. Ensure `HR_Onboarding_Form.html` is present in the project folder (the workflow opens it via a local file path — update the path in the `Open Application`/`Target App` if you relocate the file).
4. Run `Main.xaml`.
5. Follow the Input Dialog prompts to enter employee details.
6. View the generated Employee ID and Email in the final Message Box.

## Notes

- The HTML form is a **local, self-contained simulation** — no real backend or database is involved. Employee ID and email generation happen entirely client-side in JavaScript.
- Because the ID includes a random component, IDs are not guaranteed to be unique across runs (a real HR system would enforce uniqueness via a database).
- This project is intended as a learning/demo example for UiPath web automation, form filling, and data extraction techniques.

## Author

Built by Khaled as part of hands-on UiPath RPA practice.
