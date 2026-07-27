# Amazon Textract - OCR & Invoice Processing

A comprehensive guide to Amazon Textract, its capabilities, supported operations, file formats, JSON output, pricing, and best practices.

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Amazon Textract?](#2-what-is-amazon-textract)
3. [How Amazon Textract Works](#3-how-amazon-textract-works)
4. [Operations Supported by Amazon Textract](#5-operations-supported-by-amazon-textract)
    - 4.1 [DetectDocumentText](#51-detectdocumenttext)
    - 4.2 [AnalyzeDocument](#52-analyzedocument)
    - 4.3 [AnalyzeExpense](#53-analyzeexpense)
    - 4.4 [AnalyzeID](#54-analyzeid)
    - 4.5 [AnalyzeLending](#55-analyzelending)
5. [Features Supported](#6-features-supported)
6. [What Amazon Textract Does NOT Support](#7-what-amazon-textract-does-not-support)
7. [Supported File Formats](#8-supported-file-formats)
8. [JSON Output](#9-json-output)
9. [Common Use Cases](#10-common-use-cases)
10. [Advantages](#11-advantages)
11. [Limitations](#12-limitations)
12. [Pricing](#13-pricing)
13. [Practical Implementation Using AWS Console](#13-practical-implementation-using-aws-console)
---

# 1. Introduction

Amazon Textract is a fully managed AI-powered OCR service provided by AWS that automatically extracts text and structured information from documents.

It goes beyond traditional OCR by identifying:

- Printed text
- Tables
- Forms
- Invoices
- Receipts


---

# 2. What is Amazon Textract?

Amazon Textract is an AWS Machine Learning service that extracts information from scanned documents and converts it into structured JSON.

Unlike traditional OCR engines, Textract understands document structure and relationships between different fields.

Typical use cases include:

- Invoice Processing
- Receipt Processing
- Banking Documents


---

# 3. How Amazon Textract Works

<img width="1179" height="397" alt="Untitled-2026-07-27-1136" src="https://github.com/user-attachments/assets/b85561de-819c-4cd3-a02d-86b0274dc901" />

Textract first performs OCR to extract text and then applies AI models to understand the document structure.

---


# 4. Operations Supported by Amazon Textract

## 4.1 DetectDocumentText

Extracts raw printed and handwritten text.

### Returns

- Words
- Lines
- Confidence Score

---

## 4.2 AnalyzeDocument

Extracts structured information from:

- Forms
- Tables
- Checkboxes
- Signatures
- Queries
- Layout

---

## 4.3 AnalyzeExpense

Specialized AI model for invoices and receipts.

Automatically extracts:

- Vendor Name
- Vendor Address
- GST Number
- Invoice Number
- Invoice Date
- Purchase Order Number
- Currency
- Tax
- Line Items
- Total Amount

---

## 4.4 AnalyzeID

Extracts information from identity documents.

Supports:

- Passport
- Driving License
- Government IDs

Returns:

- Name
- DOB
- Address
- ID Number

---

## 4.5 AnalyzeLending

Designed for mortgage and lending-related documents.

Automatically classifies and extracts lending information.

---

# 5. Features Supported

Amazon Textract supports:

- Printed Text
- Handwritten Text
- Forms
- Tables
- Key-Value Pairs
- Layout Analysis
- Signatures
- Invoices
- Receipts
- Identity Documents

---

# 6. What Amazon Textract Does NOT Support

Amazon Textract **does not decode**:

- QR Codes
- 1D Barcodes
- Data Matrix Codes
- PDF417
- Aztec Codes

These require external libraries such as:

- pyzbar
- OpenCV
- zxing-cpp

---

# 7. Supported File Formats

| File Type | Supported |
|-----------|-----------|
| JPEG (.jpg) | Yes |
| PNG (.png) | Yes |
| PDF | Yes |
| TIFF | Yes |

Notes:

- Multi-page PDFs are supported.
- Multi-page TIFF files are supported.
- Password-protected PDFs are not supported.

---

# 8. JSON Output

Important fields to notice in JSON file are defined below.

Example:

```json
{
  "ExpenseDocuments": [
    {
      "SummaryFields": [
        {
          "Type": {
            "Text": "VENDOR_NAME"
          },
          "ValueDetection": {
            "Text": "ABC Retail Pvt Ltd"
          }
        },
        {
          "Type": {
            "Text": "INVOICE_RECEIPT_ID"
          },
          "ValueDetection": {
            "Text": "GST-2026-00045"
          }
        },
        {
          "Type": {
            "Text": "TOTAL"
          },
          "ValueDetection": {
            "Text": "3540.00"
          }
        }
      ]
    }
  ]
}
```

Applications can parse this JSON to store invoice data in databases or integrate with downstream systems.

---

# 9. Common Use Cases

- Invoice Processing
- Receipt Automation
- Accounts Payable
- Banking
- Insurance
- Healthcare
- Identity Verification
- HR Automation
- Legal Document Processing

---

# 10. Advantages

- Fully Managed AWS Service
- No Infrastructure Management
- No Model Training Required
- High OCR Accuracy
- Supports Handwritten Text
- Native Invoice Parsing
- Structured JSON Output
- Easy API Integration
- Highly Scalable

---

# 11. Limitations

- Cannot decode QR Codes
- Cannot decode 1D Barcodes
- Cannot decode PDF417
- Cannot decode Data Matrix
- Cannot decode Aztec Codes
- Requires good image quality
- Pricing is based on pages processed

---

# 12. Pricing

Amazon Textract follows a **pay-as-you-go** pricing model.

Pricing depends on:

- API used
- Number of pages processed
- AWS Region

| API | Billing Unit |
|------|--------------|
| DetectDocumentText | Per page |
| AnalyzeDocument | Per page |
| AnalyzeExpense | Per page |
| AnalyzeID | Per page |
| AnalyzeLending | Per page |


---

# 13. Practical Implementation Using AWS Console

The following practical exercise demonstrates how Amazon Textract can be used to process an invoice using the AWS Management Console.

## Step 1: Prepare the Invoice

A sample GST invoice in PDF format was created containing:


<img width="1920" height="1002" alt="image" src="https://github.com/user-attachments/assets/1528e5f7-8cdb-4e63-8137-b20ff0fe8924" />

---

## Step 2: Upload the Invoice to Amazon S3



<img width="1920" height="1002" alt="image" src="https://github.com/user-attachments/assets/4ea41f20-b490-40a4-b45f-81c965d72db6" />



---

## Step 3: Open Amazon Textract

1. Navigate to **Amazon Textract**.
2. Select **Analyze Expense**.
3. Choose **Amazon S3** as the document source.
4. Browse and select the uploaded invoice.
5. Click **Analyze**.

Textract processes the document and extracts invoice-related information.

<img width="1920" height="1002" alt="image" src="https://github.com/user-attachments/assets/30b2f457-8185-4baf-9baf-44bed722eea0" />

<img width="1920" height="1002" alt="image" src="https://github.com/user-attachments/assets/7a2a817d-ea81-4262-b1e3-b0445f37c799" />


---

## Step 4: Review the Extracted Results

The console displays structured invoice information such as:

- Vendor Name
- Invoice Number
- Invoice Date
- GST Number
- Tax Amount
- Currency
- Line Items
- Total Amount

The extracted results can also be downloaded as JSON.

<img width="1920" height="1002" alt="image" src="https://github.com/user-attachments/assets/97c44366-cdb3-4034-a919-7b35da12e766" />


---

## Step 5: Inspect the JSON Output

The generated JSON contains structured fields under:

<img width="1920" height="1002" alt="image" src="https://github.com/user-attachments/assets/b6e0e7ab-6143-4d3e-af6a-64791db73a26" />


Common fields include:

| Field | Description |
|--------|-------------|
| VENDOR_NAME | Name of the vendor |
| VENDOR_GST_NUMBER | GST Number |
| INVOICE_RECEIPT_ID | Invoice Number |
| INVOICE_RECEIPT_DATE | Invoice Date |
| SUBTOTAL | Invoice Subtotal |
| TAX | Tax Amount |
| TOTAL | Grand Total |
| CURRENCY | Invoice Currency |




---

## Step 6: QR Code Observation

Although the invoice contained a QR Code, Amazon Textract **did not decode or extract the QR code data**.

Amazon Textract currently focuses on:

- OCR
- Forms
- Tables
- Invoices
- Receipts
- Identity Documents

It **does not support** decoding:

- QR Codes
- 1D Barcodes
- Data Matrix Codes
- PDF417
- Aztec Codes

To decode QR codes or barcodes, additional processing is required using Python libraries such as:

- pyzbar
- OpenCV
- zxing-cpp

---
  
