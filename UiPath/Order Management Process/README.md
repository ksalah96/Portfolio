# Order Management Process Automation (UiPath Studio Web)

An automated end-to-end **Order Management Process** built using **UiPath Studio Web** and Microsoft Office 365 (OneDrive/Excel) integration. This RPA solution reads incoming customer orders from an Excel workbook, applies dynamic business rules for discounts, routes orders to the appropriate fulfillment departments, simulates payment processing with retry logic, checks inventory restocking, and updates the master spreadsheet automatically.

---

## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Workflow Architecture](#-workflow-architecture)
- [Business Logic & Rules](#-business-logic--rules)
- [Excel Workbook Schema](#-excel-workbook-schema)
- [Prerequisites & Setup](#-prerequisites--setup)
- [Project Structure](#-project-structure)
- [Author](#-author)

---

## 🚀 Project Overview

Manual order processing is time-consuming and prone to human error. This workflow automates the entire lifecycle of order validation and fulfillment tracking by connecting directly to an `Orders.xlsx` file stored in OneDrive. For every row in the spreadsheet, the bot orchestrates four modular sub-workflows and writes the calculated results back into the workbook.

---

## ⚙️ Workflow Architecture

The project is modularized into a main orchestrator workflow and four specialized sub-workflows:

1. **`Main.xaml`**
   - **Trigger:** Manual Trigger.
   - **Action:** Connects to OneDrive via UiPath Integration Service and iterates through each row in `Orders.xlsx` (`Sheet1`).
   - **Orchestration:** Invokes the sub-workflows in sequence, calculates the final order total (`Order Amount * (1 - Discount/100)`), and overwrites the output columns in the Excel file.

2. **`Calculate Discount.xaml`**
   - Evaluates customer loyalty tiers and order amounts using an `Else If` decision tree to determine the applicable percentage discount.

3. **`Assign Department.xaml`**
   - Uses a `Switch` activity to route each order to its designated fulfillment department based on the item's category.

4. **`Payment Processing.xaml`**
   - Simulates a payment gateway transaction using a `Do While` retry loop (up to 3 attempts) with an 80% simulated success rate per attempt.

5. **`Check inventory.xaml`**
   - Simulates inventory availability and restocking checks using a `While` loop (up to 3 days/iterations) with a 70% probability of restocking per cycle.

---

## 🧠 Business Logic & Rules

### 1. Discount Calculation (`Calculate Discount.xaml`)
| Customer Tier | Order Amount Condition | Discount Applied |
| :--- | :--- | :---: |
| **Gold** | $\ge \$1,000$ | **15%** |
| **Silver** | $\ge \$1,000$ | **10%** |
| **Any Tier** | $\ge \$1,000$ | **5%** |
| **Any Tier** | $< \$1,000$ | **0%** |

### 2. Department Routing (`Assign Department.xaml`)
| Item Category | Assigned Department |
| :--- | :--- |
| `Electronics` | Electronics Fulfillment |
| `Clothing` | Apparel Fulfillment |
| `Grocery` | Grocery Fulfillment |
| *Default / Other* | General Fulfillment |

### 3. Payment & Inventory Loops
- **Payment Processing:** Attempts to process payment up to **3 times** until `out_PaymentStatus` equals `"Success"`. If all 3 attempts fail, the status is recorded as `"Failed"`.
- **Inventory Check:** Checks stock status for up to **3 simulated days** if the item is not yet `"Available"`, updating the status to either `"Available"` or `"BackOrder"`.

---

## 📊 Excel Workbook Schema

The automation expects an Excel workbook named **`Orders.xlsx`** (`Sheet1`) with the following columns:

### Input Columns (Pre-populated)
- `Order ID`
- `Customer Name`
- `Customer Tier` *(e.g., Gold, Silver, Bronze)*
- `Item Category` *(e.g., Electronics, Clothing, Grocery)*
- `Order Amount`
- `Current Inventory Status` *(e.g., Available, BackOrder)*

### Output Columns (Populated by Robot)
- `Discount`
- `Total`
- `Department`
- `Payment Status`
- `Updated Inventory Status`

---

## 🛠️ Prerequisites & Setup

To run or modify this project in **UiPath Studio Web** (or UiPath Studio Desktop):

1. **UiPath Cloud Account:** Access to [UiPath Automation Cloud](https://cloud.uipath.com/).
2. **Microsoft OneDrive / Office 365 Connection:** 
   - Configure the **Microsoft OneDrive & SharePoint** connector in UiPath Integration Service.
   - Upload the sample `Orders.xlsx` file to your OneDrive and re-link the file picker in the **For Each Row in Workbook** and **Write Row** activities inside `Main.xaml`.
3. **Dependencies:**
   - `UiPath.System.Activities`
   - `UiPath.MicrosoftOffice365.Activities`

---

## 📁 Project Structure

```text
Order-Management-Process/
├── .entities/                  # Generated typed data row entities for Excel
├── .local/                     # Local project metadata
├── .project/                   # UiPath Studio Web project settings
├── Assign Department.xaml      # Department routing logic (Switch)
├── Calculate Discount.xaml     # Tier-based discount logic (Else If)
├── Check inventory.xaml        # Inventory restock simulation (While Loop)
├── Main.xaml                   # Main orchestrator workflow
├── Payment Processing.xaml     # Payment retry simulation (Do While Loop)
├── entry-points.json           # Workflow entry point definitions
├── project.json                # Project dependencies and configuration
├── project.uiproj              # UiPath project descriptor
└── README.md                   # Project documentation
```

---

## 👨‍💻 Author

**Khaled Salah**