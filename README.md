# LMV-RPA Project

## Overview

The LMV-RPA project is a Python-based tool for Optical Character Recognition (OCR) and data extraction from invoices using multiple OCR models. The system integrates several popular OCR libraries including PaddleOCR, TesseractOCR, DoctrOCR, and EasyOCR, and utilizes Google's Gemini AI model to extract structured data from unstructured invoice text.

## Dependencies

The following libraries are required for running the project:

- PaddlePaddle and PaddleOCR
- Tesseract OCR
- Doctr OCR (with PyTorch backend)
- EasyOCR
- Google Gemini API for text processing

Step 1: Install the necessary libraries using the following commands:

```bash
pip install paddlepaddle paddleocr
pip install pytesseract
pip install python-doctr[torch,viz]
pip install easyocr
pip install transformers
pip install diffusers
pip install bitsandbytes
pip install -U transformers accelerate bitsandbytes

```

Step 2: Define OCR Functions
Functions for each OCR library are defined to process image files and extract text:

paddle_ocr: Uses PaddleOCR to extract text.
tesseract_ocr: Uses Tesseract OCR to extract text.
doctr_ocr: Uses Doctr OCR with a PyTorch backend.
easy_ocr_func: Uses EasyOCR to extract text.
Example function for paddle_ocr:

```bash
def paddle_ocr(image_path):
    ocr = PaddleOCR(use_angle_cls=True, lang='en')  # English language model
    result = ocr.ocr(image_path)
    extracted_text = ''
    for line in result:
        extracted_text += ' '.join([word_info[1][0] for word_info in line]) + '\n'
    return extracted_text
```

Step 3: Integrate LLM Processing
Once the OCR text is extracted, the project integrates the Google Gemini model to further process the extracted data:
```bash
genai.configure(api_key=userdata.get("74916961943"))
```

Step 4: Generate Content from Extracted Text
Once the text is extracted using OCR, you can generate structured content in JSON format using the generate_gemini_content function. It processes the OCR results through the Gemini model:

```bash
def generate_gemini_content(paddle_text, tesseract_text, doctr_text, easyocr_text, model):
    # Generate content using the provided model
    paddle_gemini = model.generate_content(f'{paddle_text}')
    tesseract_gemini = model.generate_content(f'{tesseract_text}')
    doctr_gemini = model.generate_content(f'{doctr_text}')
    easyocr_gemini = model.generate_content(f'{easyocr_text}')

    # Return the generated content as a dictionary
    return {
        'PaddleGemini': paddle_gemini.text,
        'TesseractGemini': tesseract_gemini.text,
        'DoctrGemini': doctr_gemini.text,
        'EasyOCRGemini': easyocr_gemini.text
    }
```
## Usage
Upload an invoice image or provide a path to an image file in your local system.
Call the extract_text_from_ocr(image_path) function to extract text using all OCR models.
The function will return a dictionary containing the OCR results from each model.
Use the generate_gemini_content function to process the extracted text with the Gemini model.
Example usage:
