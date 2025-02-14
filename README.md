# ERC Transaction Matcher

A Python-based tool for matching and validating Employee Retention Credit (ERC) transactions against Form 941-X filings.

## Overview

This project processes two main types of documents:
1. ERC transaction data showing claimed credits
2. Form 941-X tax filings containing reported line 27 amounts

The tool matches and compares these data sources to identify potential discrepancies between claimed credits and filed amounts.

## Project Structure

The project consists of five main processing steps:

### Step 1: Transaction Processing
- Processes transaction files containing ERC claims
- Creates quarter_year column for temporal matching
- Pivots data into quarter-based format

### Step 2: Form 941-X Processing 
- Extracts data from Form 941-X PDFs
- Processes both "good" (standard format) and "bad" (non-standard format) PDFs
- Uses OCR for extracting line 27 values from PDF forms

### Step 3: Line 27 Extraction
- Specifically targets line 27 values from Form 941-X PDFs
- Uses targeted OCR with specific coordinates
- Includes visualization for debugging

### Step 4: Data Joining
- Combines transactions with Form 941-X data
- Creates quarter-based columns for comparison
- Normalizes data formats

### Step 5: Final Output
- Produces final ERC comparison file
- Includes both claimed amounts and filed amounts by quarter
- Calculates totals for comparison

## Key Files

- `Transaction-Matcher-2025.pdf`: Main processing script
- `imperfect_941.csv`: Combined 941-X data
- `perfect_941.csv`: Cleaned and formatted 941-X data
- `erc_01-30-25.csv`: Final output file with matched transactions

## Dependencies

- pandas
- numpy
- pdfplumber
- cv2 (OpenCV)
- pytesseract
- pdf2image
- coconut
- siuba

## Usage

The main processing flow is executed in sequence:

1. Process transaction data:
```python
transactions_proc = process_transactions(input_file)
```

2. Process 941-X forms:
```python
df_941s_combined = process_941_forms(pdf_folder)
```

3. Extract line 27 values:
```python
df_line27 = extract_line27(pdf_folder)
```

4. Join and finalize data:
```python
erc = create_final_output(transactions_proc, perfect_941)
```

## Output Format

The final output (`erc_01-30-25.csv`) contains the following key columns:

- taxpayer_name
- tin (Tax ID)
- refund_total
- filing_total
- Quarterly columns for both claimed and reported amounts:
  - refunds_Q{X}_{YEAR}
  - erc_claimed_Q{X}_{YEAR}