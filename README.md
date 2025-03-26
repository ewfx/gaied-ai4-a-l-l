# Email Classification Script

This repository contains a script `emailclassify.py` that performs various operations on emails, including duplicate detection, OCR text extraction from attachments, email classification, primary intent detection, and context-based data extraction for banking-related information.

## Prerequisites

1. **Python 3.6+**: Ensure you have Python installed on your system.
2. **Required Python Packages**:
    - `pytesseract`
    - `pdf2image`
    - `openai`
    - `email`
    - `typing`

You can install the required packages using the following command:
```sh
pip install pytesseract pdf2image openai
```

3. **Tesseract OCR**: Make sure Tesseract OCR is installed and properly configured on your system. [Tesseract Installation Guide](https://github.com/tesseract-ocr/tesseract)

4. **OpenAI API Key**: You need an OpenAI API key to use the GPT-4 model for email classification. Replace `"your_openai_api_key"` with your actual API key in the script.

## Script Overview

The `emailclassify.py` script includes the following functions:

- `get_email_hash(email_body: str) -> str`: Generates a SHA-256 hash of the email body to detect duplicates.
- `extract_text_from_attachment(file_path: str) -> str`: Extracts text from PDF attachments using OCR.
- `classify_email(email_body: str) -> Dict[str, Any]`: Classifies the email into request types and sub-requests using GPT-4.
- `detect_primary_intent(email_body: str) -> Dict[str, Any]`: Detects the primary intent and prioritizes requests in the email using GPT-4.
- `extract_key_fields(email_body: str) -> Dict[str, Any]`: Extracts key banking-related fields from the email using GPT-4.
- `process_email(email_text: str, attachment_path: str = None, rules: Dict[str, Any] = None) -> Dict[str, Any]`: Main function that processes the email and returns structured results.

## Usage

1. **Replace OpenAI API Key**:
    Open the `emailclassify.py` file and replace `"your_openai_api_key"` with your actual OpenAI API key.

2. **Run the Script**:
    You can run the script directly using the following command:
    ```sh
    python emailclassify.py
    ```

3. **Sample Email Processing**:
    The script contains an example usage section that demonstrates how to process a sample email. You can modify this section to test with your own email text and attachment paths.

## Example

```python
if __name__ == "__main__":
    sample_email = """
    Subject: Urgent - Loan Application Status & Credit Card Limit Increase
    Dear Bank,
    I would like to check the status of my loan application for $50,000 with an interest rate of 5.5%. My application ID is 123456.
    Additionally, I need to request an increase in my credit card limit to $20,000.
    Regards,
    Alice
    """
    result = process_email(sample_email, attachment_path=None)
    print(result)
```

## Output

The `process_email` function returns a structured result containing the following information:

- `email_hash`: Hash of the email body.
- `classification`: Classification of the email into request types and sub-requests.
- `primary_intent`: Detected primary intent and prioritization of requests.
- `extracted_fields`: Extracted key banking-related fields.
- `attachment_text`: Extracted text from the attachment (if any).
- `duplicate_flag`: Hash of the email body for duplicate detection.
- `duplicate_reason`: Reason for potential duplicate detection.

## Contributing

Feel free to contribute to this project by submitting issues or pull requests. For major changes, please open an issue to discuss what you would like to change.

## License

This project is licensed under the MIT License.
