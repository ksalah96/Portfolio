# Add New Vendor Details

A UiPath RPA project that automates logging into the ACME website and adding a new vendor through its web UI.

## Overview

This robot logs in to the ACME system, navigates to the vendor management area, and creates a new vendor record by filling in the vendor's details and saving the entry. It's built with UiPath Studio using the modern (Screen/App) automation framework and Chrome-based UI Automation activities.

## Workflow Structure

| File | Purpose |
|---|---|
| `Main.xaml` | Entry point. Checks whether the user is already logged in (via `Find Element - Login` / `Find Element - Dashboard`) and branches accordingly, then invokes the login, navigation, and vendor-creation workflows in sequence. |
| `Login_To_Acme_Website.xaml` | Requests credentials, logs in to the ACME website, verifies success via `Check App State`, and terminates the workflow if login fails. |
| `Navigate_To_Add_Vendor.xaml` | From the dashboard, clicks **Vendors** and then **Add Vendor** to reach the new-vendor form. |
| `Add_a_New_Vendor.xaml` | Fills in the vendor **Name**, **Address**, **City**, **Country**, and **Vendor Tax ID** fields and clicks **Save Vendor**. |

### Process Flow

```
Main
 ├─ Check App State (logged in?)
 │
 ├─ PickBranch: User not Logged In
 │   └─ Login To Acme Website
 │        ├─ Request Credentials
 │        ├─ Type Into 'Email' / 'Password'
 │        ├─ Click 'Login'
 │        └─ Check App State - Logging Successful (else Terminate Workflow)
 │
 ├─ PickBranch: User Logged In
 │
 ├─ Navigate To Add Vendor
 │   ├─ Click 'Vendors'
 │   └─ Click 'Add Vendor'
 │
 └─ Add a New Vendor
      ├─ Type Into 'Name' / 'Address' / 'City' / 'Country' / 'Vendor TaxID'
      └─ Click 'Save Vendor' → Click 'OK'
```

## Key Variables

- `UserName`, `Password` (SecureString) — ACME login credentials, retrieved via **Request Credentials**
- `VendorName`, `VendorAddress`, `VendorCity`, `VendorCountry`, `VendorID` — the new vendor's details entered into the form

## Prerequisites

- [UiPath Studio](https://www.uipath.com/product/studio) (compatible with the modern/UIAutomationNext activity set)
- UiPath.UIAutomation.Activities package
- Google Chrome with the UiPath extension installed
- Access to the ACME test website (`acme-test.uipath.com`) with valid credentials
- Credentials stored in a UiPath Orchestrator/Assistant credential store (or the Windows Credential Manager) matching the alias used by **Request Credentials**

## How to Run

1. Open `project.uiproj` in UiPath Studio (or open this folder as a project).
2. Ensure Chrome and the UiPath extension are installed and enabled.
3. Set up the ACME login credentials in the credential store expected by `Login_To_Acme_Website.xaml`.
4. Run `Main.xaml`, or execute the process via UiPath Assistant.

## Project Structure

```
Add New Vendor Details/
├── Main.xaml
├── Login_To_Acme_Website.xaml
├── Navigate_To_Add_Vendor.xaml
├── Add_a_New_Vendor.xaml
├── project.json
├── project.uiproj
├── entry-points.json
├── .entities/
├── .objects/
├── .screenshots/
├── .settings/
├── .storage/
├── .templates/
└── .tmh/
```

## Notes

- This project was built as a training/practice exercise on the UiPath Automation Developer learning path, targeting the ACME test environment.
- The UI targeting selectors are specific to the ACME test site layout; if the site's UI changes, the `Chrome ACME System 1 - *` UI Automation activities and their targets may need to be re-recorded.

---

**Author:** Khaled Salah
