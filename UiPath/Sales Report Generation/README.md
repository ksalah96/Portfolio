# RPA Challenge - Automated Sales Report Generator & PDF Converter

An automated Robotic Process Automation (RPA) workflow built using **UiPath Studio**. This project reads employee sales data from a CSV file, dynamically populates a Word document template for each employee, launches a custom conversion application, and UI-automates the conversion of the generated Word documents into PDF format.

---

## 📋 Features

* **CSV Data Processing**: Reads and parses tabular employee sales data (`Input_Data.csv`) into a DataTable.
* **Dynamic Template Filling**: Uses Word Automation Activities to populate placeholders (`<Name>`, `<Division>`, `<Sales>`) inside `Master_Template.docx` and saves individual reports under `Word_Files/`.
* **Application Execution**: Automates the launch of an external executable (`WordToPDFConverterApp.exe`).
* **UI Automation**: Iterates through generated Word documents, interacts with the desktop UI of `WordToPDFConverterApp`, sets input/destination paths, and converts each document into a PDF stored in `PDF_Files/`.

---

## 🛠️ Tech Stack & Dependencies

* **RPA Platform**: UiPath Studio
* **Activities Packages**:
  * `UiPath.System.Activities`
  * `UiPath.Excel.Activities` (CSV activities)
  * `UiPath.Word.Activities`
  * `UiPath.UIAutomationNext.Activities`
* **External Application**: `WordToPDFConverterApp.exe`

---

## 📁 Repository Structure

```text
├── Word_Files/                # Output folder for generated .docx reports
├── PDF_Files/                 # Output folder for converted .pdf files
├── Input_Data.csv             # Input dataset (Employee Name, Division, Sales)
├── Master_Template.docx       # Template document containing placeholders
├── WordToPDFConverterApp.exe  # Converter desktop application
├── Main.xaml                  # Main UiPath flowchart process workflow
├── project.json               # UiPath project dependencies & configuration
└── README.md                  # Project documentation
```

---

## ⚙️ How It Works (Workflow Overview)

1. **Read Input Data**: Workflow reads `Input_Data.csv` using the **Read CSV** activity.
2. **Generate Word Documents**:
   * Loops through each row in the dataset.
   * Copies `Master_Template.docx` to `Word_Files/<Employee_Name>.docx`.
   * Replaces placeholders (`<Name>`, `<Division>`, `<Sales>`) with actual row values.
3. **Launch Converter App**: Invokes system process to launch `WordToPDFConverterApp.exe`.
4. **Automate PDF Conversion**:
   * Scans `Word_Files/` directory for generated `.docx` files.
   * Uses UI Automation to open files inside the converter, set output folder to `PDF_Files/`, click **Convert**, and handle confirmation prompts.

---

## 🚀 Getting Started

### Prerequisites

1. **UiPath Studio** installed (Community or Enterprise).
2. **Microsoft Word** installed (for Word Application Scope activities).
3. **Windows OS** (required for desktop UI automation).

### Execution Steps

1. Clone this repository to your local machine:
   ```bash
   git clone https://github.com/YOUR_USERNAME/RPA-Challenge.git
   ```
2. Open `project.json` in **UiPath Studio**.
3. Ensure all project dependencies are restored.
4. Verify directory permissions for reading/writing inside the project folder.
5. Click **Run** or press `F5` in UiPath Studio to execute `Main.xaml`.

---

## Author

Built by Khaled as part of hands-on UiPath RPA practice.
