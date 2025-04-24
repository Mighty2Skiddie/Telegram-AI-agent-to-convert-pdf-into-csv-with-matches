# Telegram-AI-agent-to-convert-pdf-into-csv-with-matches
# 📦 CrowdWisdomTrading Assessment - Telegram RFQ OCR Workflow

## 👋 Introduction
This workflow is built for CrowdWisdomTrading's intern assessment. It automates the process of:

- Receiving RFQ (Request for Quotation) PDFs via Telegram
- Extracting product text using OCR
- Matching it to a static product catalog using LLM via OpenRouter
- Generating a CSV of matched results
- Sending the result back to the Telegram user

## 🧠 Tech Stack Used
- **n8n** (low-code workflow automation)
- **Telegram Bot** (via BotFather)
- **OCR.Space API** (for PDF to text)
- **OpenRouter API** (for GPT-3.5-based matching)
- **CSV Generator** (via Spreadsheet File node)

## 🔁 Workflow Overview

1. **Telegram Trigger**: Catches incoming PDF
2. **Telegram Get File**: Retrieves the file path
3. **HTTP Request**: Downloads the file as binary
4. **Set Node**: Fixes file name and MIME type
5. **OCR API**: Sends file to OCR.Space
6. **Set Node**: Parses and cleans OCR result
7. **OpenRouter API**: Sends prompt to GPT-3.5 with product catalog
8. **Set Node**: Stores parsed matches
9. **Split Out Items**: Breaks match array into individual rows
10. **Spreadsheet File**: Creates binary CSV
11. **Telegram SendDocument**: Sends back the matched results

## 📁 Files Included
- `workflow.json`: Full working n8n flow
- `demo-video.mp4`: Screencast of the working workflow
- `README.md`: You’re reading it!

## ✅ Features
- Accepts any RFQ in PDF format
- Can handle multiple product lines in the PDF
- Smart product matching using LLM
- Supports automatic CSV generation and delivery

## ❗ Challenges Faced
- OCR returning text blocks without structure → handled via LLM prompt
- Telegram file download required exact file_path parsing
- OCR.Space errors due to missing file extension → fixed with Set node for `.pdf`
- LLM output was raw text → parsed with `JSON.parse()` inside n8n Set node
- Working with Non-English PDFs (Hebrew OCR)

Problem: Most OCR and language models struggle with right-to-left Hebrew text.

Solution: Used OCR.space for accurate text extraction and translated Hebrew to English using OpenRouter LLMs, allowing consistent downstream processing.

Inconsistent PDF Table Formats

Problem: RFQs came in varied layouts, especially in table structure and column positions.

Solution: Leveraged OpenRouter to parse raw OCR text into structured line-by-line product data using natural language instructions.

Vector Database Integration Issues (Qdrant)

Problem: Qdrant threw errors (400/403) due to strict format requirements and parsing issues with embedded vectors.

Solution: Switched to Pinecone, which offered simpler APIs and more forgiving error handling, making integration straightforward.

Embedding Integration in n8n

Problem: HuggingFace output could not be used directly inside n8n’s request body without causing formatting issues.

Solution: Used expression mode with exact JSON path references and configured the HTTP Request node in raw JSON mode to handle vector payloads correctly.

Matching Confidence Threshold

Problem: Some low-relevance matches were being returned due to minor vector similarities.

Solution: Applied a confidence filter in n8n using IF nodes to pass only matches with similarity scores above 0.8.

Generating and Returning Dynamic CSV Files

Problem: Creating downloadable CSVs on the fly from matched items was complex inside n8n.

Solution: Used a Code node to format CSV strings and convert them to binary data, then sent them as Telegram documents.

End-to-End Telegram Integration

Problem: Needed the process to be fully automated and user-friendly for non-technical users.

Solution: Built the entire workflow to be triggered from Telegram, where users can send a PDF and receive back a CSV with matched catalog items without interacting with any other system

## 🎁 Bonus Ideas (Optional/Stretch Goals)
- Use Supabase for storing the product catalog as a vector database
- Add language translation (Hebrew RFQ to English)
- Enable “chat with the invoice” LLM flow
- Add webhook to log RFQ history to Google Sheets

## 📤 Submission

---

https://github.com/user-attachments/assets/cceabb0d-25bb-43bb-9740-fbe61442f053


🧠 Built with AI, automation, and creative hustle!


