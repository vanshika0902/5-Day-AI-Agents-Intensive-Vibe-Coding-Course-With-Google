---
name: pdf-processing
description: Extracts text from PDF files using PyPDF2.
---

# PDF Processing Skill

## When to use this skill
Use this skill when a user needs to extract text from a PDF file.

## How to use this skill
The skill provides the extract_text() function from the parse_pdf.py script. Import it into your agent script:

python
from skills.pdf_parsing.parse_pdf import extract_text

results = extract_text(
  file_path = "/path/to/document.pdf",
  pages = "all" or "1-3" or "1,2,3"
  )

  ### Parameters

- file path (str) : Path to the PDF file
- pages: Pages to extract - all, range or specific page

## Returns

-  `success` (bool): Whether extraction succeeded
- `file_path` (str): Path to the processed file
- `total_pages` (int): Total pages in PDF
- `extracted_pages` (int): Number of pages extracted
- `pages` (list): Array of {page: number, text: string} objects
