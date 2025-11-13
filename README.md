# OCR CV Pipeline

A practical computer vision pipeline for extracting structured information from scanned documents like invoices and ID cards. It combines convolutional neural networks (CNN) for region detection with OCR for text extraction.

[![Python](https://img.shields.io/badge/Python-3.10-blue)](https://www.python.org/) [![OpenCV](https://img.shields.io/badge/OpenCV-4.x-red)](https://opencv.org/) [![FastAPI](https://img.shields.io/badge/FastAPI-API-green)](https://fastapi.tiangolo.com/) [![Tesseract](https://img.shields.io/badge/Tesseract-OCR-yellow)](https://github.com/tesseract-ocr/tesseract)

## Why This Exists  
Manual data entry from invoices and IDs is error‑prone and time‑consuming. This project provides a reproducible approach to automate that process with minimal setup.

## What It Does
- Preprocesses images (resize, normalize)
- Detects regions of interest (ROIs) using a trained CNN
- Applies OCR on each ROI to extract text
- Outputs structured JSON with fields and values

## Tech Stack
- Python
- TensorFlow or PyTorch (for CNN model)
- OpenCV
- Tesseract OCR (via pytesseract)
- FastAPI (optional API wrapper)

## Architecture
```
┌────────────────┐
│   Images     │
└───────┬──────┘
        │  Preprocess (resize, grayscale, etc.)
        ▼
┌────────┘
│   CNN Model   │ — Detect ROIs (bounding boxes)
└──────┘
        │
        ▼
┌───────┘
│   Crop ROIs   │
└──────┘
        │
        ▼
┌───────┘
│     OCR       │ — Extract text from each crop
└──────┘
        │
        ▼
┌───────┘
│ Structured    │ — Assemble JSON output
│    Output     │
└─────────┘
```

## Getting Started

1. Clone the repo and install dependencies:

```bash
git clone https://github.com/your-username/ocr-cv-pipeline
cd ocr-cv-pipeline
pip install -r requirements.txt
```

2. Copy `.env.example` to `.env` and set paths if needed (e.g. `TESSDATA_PREFIX`).

3. Run the demo script on sample images:

```bash
python scripts/process_image.py --image_path ./data/sample_invoice.jpg
```

This prints a JSON with extracted fields.

## Docker

Build and run with Docker:

```bash
make docker-build
make docker-run
```

The service exposes a FastAPI endpoint at `http://localhost:8000/process` where you can POST an image.

## Example API Usage

```bash
curl -X POST "http://localhost:8000/process" \
     -H "Content-Type: multipart/form-data" \
     -F "file=@./data/sample_invoice.jpg"
```

Returns:

```json
{
  "invoice_number": "INV-12345",
  "date": "2025-01-31",
  "total": "$1,234.56"
}
```

## Real Use Cases
- Automating invoice intake for accounting and expense management
- Verifying ID cards during customer onboarding (KYC)
- Digitizing paper records into searchable archives

## Features
- Configurable CNN backbone
- Pluggable OCR engine (default Tesseract)
- Batch processing script
- Simple API with FastAPI
- Tests for key components

## Future Improvements
- Train a better detection model on more document types
- Add field‑level validation and corrections
- Provide a lightweight web UI for uploading images
- Support handwriting recognition

## License
MIT — see LICENSE for details.
