# Automated Invoice Reporting with UiPath (RPA)

![Tags](https://img.shields.io/badge/RPA-UiPath-blue) ![Tags](https://img.shields.io/badge/AI-Document%20Understanding-green) ![Tags](https://img.shields.io/badge/AI-GenAI-purple)

An intelligent automation solution that ingests raw invoice files, extracts key data, applies business logic, and generates an actionable report for finance managers. This project demonstrates a robust, real-world approach to solving common operational bottlenecks in finance departments.

## Business Problem

Finance teams manually track invoices, a process that is slow, prone to data entry errors, and diverts skilled analysts to low-value clerical work. The specific request was for an automated report of all invoices due on or before September 22, 2025, complete with a concise summary of each purchase.

## The Solution

I designed and deployed an RPA workflow in UiPath that automates the end-to-end process. The bot systematically processes a directory of invoices, validates the data, and populates a Google Sheet with clean, actionable information for managers.

### Key Features

* **Automated Data Extraction**: Leverages UiPath's **Document Understanding** to ingest unstructured invoice files (PDFs, scans) and extract key fields like Invoice Number, Supplier, Due Date, and Total Amount.
* **Intelligent Content Generation**: Integrates **GenAI** to analyze invoice contents and generate a concise, human-readable purchase description (max 15 words), allowing managers to understand the context of a purchase without opening the file.
* **Business Rule Implementation**: The core logic filters invoices based on the manager's request, only processing those with a due date on or before September 22, 2025 for inclusion in the final report.
* **Advanced Validation & Triage Logging**: The workflow intelligently handles exceptions. Based on project feedback, an `Else` branch was added to triage invoices that don't meet the primary date criteria. 
    * It checks for low confidence in the extracted `Due Date`, flagging unreadable or poor-quality documents.
    * It also logs invoices that are perfectly readable but have a future due date, providing a complete audit trail for all processed files.
* **Automated Reporting**: The final, validated data is written to a centralized Google Sheet, creating an accurate, on-demand report for the finance team. 

## Tech Stack

* **RPA Platform**: UiPath Web Studio
* **AI Services**: UiPath Document Understanding, UiPath GenAI Activities (Claude 3.5 Sonnet)
* **Data Storage**: Google Drive, Google Sheets

## Workflow Logic

The automation follows a systematic, resilient process for each file found in the input folder:

1.  **For Each File**: The bot iterates through every file in the designated 'invoices' Google Drive folder.
2.  **Extract Data**: It uses the `Extract Document Data` activity to process the file with the pre-defined `Invoices` extractor.
3.  **Primary Due Date Check (If/Then)**:
    * The bot checks if the extracted `DueDate` is less than or equal to `2025-09-22`.
    * **If TRUE (Then Path)**: The invoice meets the criteria. It proceeds to generate the AI purchase description and writes the complete, validated row to the Google Sheet report. [cite_start]A nested check also flags if the `TotalAmount` has low confidence.
4.  **Exclusion & Triage Logic (Else)**:
    * **If FALSE (Else Path)**: The invoice does not meet the primary criteria. Instead of being ignored, it enters a triage workflow:
        * **It first checks the `DueDate.Confidence`**. If confidence is low (< 0.7), it logs a message that the invoice needs review because the date is not legible.
        * If the date confidence is high, it then checks if the `DueDate` is after the threshold and logs a message explaining that the invoice was not added to the report because its due date is in the future.

## Value Added

This solution transforms a manual, error-prone task into a streamlined, intelligent process.

* **Process Optimization & ROI**: Eliminates hours of manual data entry, freeing skilled financial analysts to focus on strategic work instead of clerical tasks.
* **Enhanced Data Accuracy**: Drastically reduces the risk of human error and uses a confidence check to flag potentially incorrect data for review, ensuring more reliable reporting.
* **Accelerated Decision-Making**: Provides managers with an instant, easy-to-understand report with AI-generated context, enabling faster payment approvals and better financial visibility.
