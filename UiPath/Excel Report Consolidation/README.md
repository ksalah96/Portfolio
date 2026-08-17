# Excel Report Consolidation

A UiPath RPA workflow that automatically consolidates multiple daily Excel sales reports into a single master workbook, then generates a pivot table and chart summarizing the results.

## What it does

Each day, this bot:

1. **Deletes** any previous `SalesResults-<date>.xlsx` file for the current day (fresh start / idempotent run).
2. **Creates/opens** a new `SalesResults-<date>.xlsx` file and writes formatted column headers:
   - `Item`
   - `Unit Price ($)`
   - `Items Sold`
   - `Total Income ($)`
3. **Loops through every file** in the `Daily Reports` folder:
   - Opens each report.
   - Checks whether its sheet is named `Sheet1`; if not, renames it and logs the correction (keeps downstream steps consistent regardless of how the source file was saved).
   - Appends that report's data into the master `SalesResults` workbook.
4. **Auto-fits** columns/rows in the consolidated sheet for readability.
5. **Builds a pivot table** (`SalesByStore`) on `Sheet2`, summing `Total Income ($)` grouped by `Item`.
6. **Inserts a pie chart** next to the pivot table visualizing income share by item.

The result is a single, dated, formatted workbook containing all daily reports plus a ready-made summary view — no manual copy-pasting required.

## Project structure

```
Excel Report Consolidation/
├── Main.xaml            # Main workflow (entry point)
├── project.json          # UiPath project manifest/dependencies
├── project.uiproj        # UiPath project file
├── Daily Reports/        # Input: individual daily sales report .xlsx files
├── Sales Results/        # Output: consolidated SalesResults-<date>.xlsx file
├── .objects/              # UiPath-generated metadata
├── .entities/             # UiPath-generated metadata
├── .settings/             # UiPath-generated metadata
├── .templates/            # UiPath-generated metadata
└── .tmh/                  # UiPath-generated metadata
```

## Requirements

- [UiPath Studio](https://www.uipath.com/product/studio) (Windows)
- Microsoft Excel installed locally (workflow uses the Excel desktop application, not the Excel Interop-free/background mode)
- UiPath activity packages:
  - `UiPath.Excel.Activities`
  - `UiPath.System.Activities`

## Setup

1. Clone this repo into your UiPath workspace, or open `project.json` directly in UiPath Studio.
2. Confirm the folder paths match your environment (defaults reference a local `Daily Reports` and `Sales Results` folder relative to the project directory — update the hardcoded path in the **For Each Excel Report File in Folder** activity if your folder is elsewhere).
3. Drop each day's individual sales report `.xlsx` files into `Daily Reports/`. Each report is expected to have a `Sheet1` containing columns matching the headers above (`Item`, `Unit Price ($)`, `Items Sold`, `Total Income ($)`).
4. Run `Main.xaml`.

## Output

A new file named:

```
Sales Results/SalesResults-YYYY-MM-DD.xlsx
```

is generated, containing:
- **Sheet1** — all consolidated report rows with formatted headers.
- **Sheet2** — a pivot table (`SalesByStore`) and pie chart summarizing total income by item.

## Notes

- The workflow deletes and regenerates the current day's `SalesResults` file each run, so re-running it on the same day is safe and won't duplicate data.
- If a source report's sheet isn't named `Sheet1`, the bot automatically renames it and logs the event rather than failing.

## License

Add a license of your choice (e.g. MIT) if you intend to share or reuse this publicly.

## Author

Khaled Salah
