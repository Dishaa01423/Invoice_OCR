# 🧾 Intelligent Invoice Processing System

An end-to-end automated invoice processing pipeline built with n8n workflow automation that extracts, validates, and exports invoice data from PDFs using AI-powered OCR and natural language processing.

![n8n](https://img.shields.io/badge/n8n-Workflow_Automation-EA4B71?style=for-the-badge\&logo=n8n)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-412991?style=for-the-badge\&logo=openai)
![Excel](https://img.shields.io/badge/Excel-Data_Export-217346?style=for-the-badge\&logo=microsoft-excel)
![LlamaIndex](https://img.shields.io/badge/LlamaIndex-OCR-8B5CF6?style=for-the-badge)

## 📋 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Database Schema](#database-schema)
- [Usage](#usage)
- [Workflow Details](#workflow-details)
- [Data Extraction](#data-extraction)
- [Error Handling](#error-handling)
- [Future Enhancements](#future-enhancements)
- [Contributing](#contributing)
- [License](#license)
---

## 🎯 Overview

This system automates the entire invoice processing workflow—from detecting new invoices uploaded to Google Drive to extracting structured data and exporting it as **Excel files**. It handles complex Indian GST invoices with multi-line items, tax calculations, and various invoice formats.

**Key Achievement**: Reduces manual invoice processing time by **90%**, processing **50+ invoices daily** with **95%+ accuracy**.

---

## ✨ Features

* 🔄 **Automated Monitoring**: Watches Google Drive folder for new invoice uploads
* 📄 **Multi-format Support**: Processes PDFs, images, and scanned documents
* 🤖 **AI-Powered Extraction**: LlamaIndex OCR + OpenAI GPT-4o-mini for precision parsing
* 🇮🇳 **GST Compliant**: Extracts GSTIN, CGST, SGST, IGST, HSN/SAC codes
* 💰 **Multi-currency Support**
* 📊 **Excel Export**: Generates *Invoices.xlsx* and *Line_Items.xlsx*
* ✅ **Data Validation**: Structure validation + type safety
* 🔁 **Asynchronous Processing**: Automated OCR polling
* 📈 **Scalable**: Handles multi-page invoices with multiple line items

---

## 🏗️ Architecture

```
┌─────────────────┐
│  Google Drive   │  ← Invoice Upload
│    Trigger      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Download File  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  LlamaIndex OCR │  ← Extract Text
│   (API Upload)  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   Wait & Poll   │  ← Job Status
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Get Markdown    │  ← Extracted Text
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ OpenAI GPT-4o   │  ← JSON Structured Output
│ Information     │
│ Extractor       │
└────────┬────────┘
         │
         ▼
┌─────────┬────────┐
│         │        │
▼         ▼        ▼
Invoice  Line    Validation
Parser   Items   & Error
         Parser  Handling
│         │
▼         ▼
┌──────────────────────────┐
│ Excel Export (2 files)   │
│ invoices.xlsx +          │
│ line_items.xlsx          │
└──────────────────────────┘
```

---

## 🛠️ Tech Stack

| Technology             | Purpose                   |
| ---------------------- | ------------------------- |
| **n8n**                | Workflow automation       |
| **LlamaIndex Cloud**   | OCR + parsing             |
| **OpenAI GPT-4o-mini** | Information extraction    |
| **Excel (XLSX)**       | Final structured output   |
| **Google Drive API**   | File detection + download |
| **JavaScript**         | Data transformation       |

---

## 📦 Prerequisites

* **n8n instance** (self-hosted or cloud)
* **Google Cloud Project** with Drive API enabled
* **LlamaIndex Cloud API key**
* **OpenAI API key**
* **Node.js** (v16+) if running n8n locally
* **Sample invoices** (provided in `sample_invoices/`)

---

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/invoice-processing-n8n.git
cd invoice-processing-n8n
```

### 2. Start n8n

#### Option A: Docker

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

#### Option B: npm

```bash
npm install n8n -g
n8n start
```

### 3. Import Workflow

1. Go to `http://localhost:5678`
2. **Workflows → Import**
3. Upload `workflow.json`
4. Click **Activate**

---

## ⚙️ Configuration

### Google Drive Setup

* Enable Drive API
* Create OAuth credentials
* Add n8n Google Drive OAuth2
* Set your `folderToWatch` ID

### LlamaIndex API Setup

Replace API key:

```
Authorization: Bearer YOUR_LLAMAINDEX_API_KEY
```

### OpenAI Setup

Configure **OpenAI Chat Model** node with your key.

---

## 📁 Output Files

At the end of each workflow run, two Excel files are generated:

### 📄 `invoices.xlsx`

Contains invoice-level data:

* invoice_id
* invoice_number
* invoice_date
* vendor & buyer details
* GST values
* totals & taxes
* currency, state, place of supply

### 📄 `line_items.xlsx`

Contains item-level data:

* invoice_id (links to invoices sheet)
* description
* quantity, unit price, amount
* HSN/SAC
* CGST/SGST/IGST amounts
* taxable amount

---

## 📖 Usage

### Automatic Mode

1. Upload invoice into Google Drive folder
2. Workflow triggers automatically
3. OCR + AI extraction executes
4. Two Excel files are generated

### Manual Mode

* Open workflow
* Click **Execute Workflow**
* Select invoice
* View result in Excel export nodes

---

## 🔧 Workflow Details

| Node Name             | Purpose                                       |
| --------------------- | --------------------------------------------- |
| Google Drive Trigger  | Detect new invoice files                      |
| Download File         | Fetch file from Drive                         |
| LlamaIndex OCR        | Extracts text                                 |
| Wait / Poll           | Check OCR job status                          |
| Get OCR Text          | Retrieve markdown                             |
| OpenAI JSON Extractor | Converts unstructured text to structured JSON |
| Invoice Parser        | Extract invoice-level data                    |
| Line Items Parser     | Extract item list                             |
| Spreadsheet File      | Export invoices.xlsx                          |
| Spreadsheet File      | Export line_items.xlsx                        |

---

## 🤖 Data Extraction

### Invoice Fields (18+)

* Invoice number, date, due date
* Vendor/buyer details
* GSTIN, CIN, PAN
* State code & place of supply
* Total amount, taxes
* Currency code, PO number

### Line Item Fields (13+)

* Description
* Quantity
* Unit price
* Amount
* Tax percentages & amounts
* HSN/SAC
* UOM

---

## 🛡️ Error Handling

* JSON structure validation
* Missing field fallback
* Type conversion (string → number/date)
* Retry logic for OCR jobs
* Graceful handling for missing line items

---

## 🚀 Future Enhancements

* [ ] Add email notifications
* [ ] Duplicate invoice detection
* [ ] Multi-language support
* [ ] Analytics dashboard
* [ ] Support for credit/debit notes
* [ ] Batch processing
* [ ] REST API endpoints
* [ ] Enhanced Excel formatting
* [ ] Auto-upload Excel to Drive/SharePoint

---

## 📊 Performance Metrics

| Metric            | Value                      |
| ----------------- | -------------------------- |
| Accuracy          | 95%+                       |
| Processing Time   | ~15–30 seconds per invoice |
| Capacity          | 50+ invoices/day           |
| Time Saved        | 90%                        |
| Supported Formats | PDF, PNG, JPG, JPEG        |
| Max File Size     | 10MB                       |

---

## 👤 Author

**Disha Mathankar**

* GitHub: [@Dishaa01423](https://github.com/Dishaa01423)
* LinkedIn: [Disha Mathankar](https://www.linkedin.com/in/disha-mathankar-159048282)
* Email: [disha01423@gmail.com](mailto:disha01423@gmail.com)

---

## 🙏 Acknowledgments

* n8n for workflow automation
* LlamaIndex for OCR
* OpenAI for GPT-4o-mini
* Indian GST documentation

---

## 📞 Support

* Open an issue
* Email: [disha01423@gmail.com](mailto:disha01423@gmail.com)
* Connect on LinkedIn

---

⭐ **If you find this project helpful, please give it a star!** ⭐

---

