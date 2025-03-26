import os
import hashlib
import pytesseract
from pdf2image import convert_from_path
from openai import OpenAI
from email.parser import Parser
from typing import List, Dict, Any

# OpenAI API setup (Replace with your API key)
client = OpenAI(api_key="your_openai_api_key")

# Duplicate Email Detection using Hashing
def get_email_hash(email_body: str) -> str:
    return hashlib.sha256(email_body.encode()).hexdigest()

# OCR Text Extraction
def extract_text_from_attachment(file_path: str) -> str:
    if file_path.endswith(".pdf"):
        images = convert_from_path(file_path)
        text = "\n".join([pytesseract.image_to_string(img) for img in images])
        return text
    return "Unsupported file type"

# Email Classification for Banking Requests and Sub-Requests
def classify_email(email_body: str) -> Dict[str, Any]:
    response = client.chat.completions.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "Classify this banking-related email into possible request types and sub-requests, along with prioritization and confidence scores."},
            {"role": "user", "content": email_body},
        ]
    )
    return response.choices[0].message.content

# Multi-Request & Primary Intent Detection
def detect_primary_intent(email_body: str) -> Dict[str, Any]:
    response = client.chat.completions.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "Identify all banking-related requests in the email, classify them into main and sub-requests, prioritize them, and provide confidence scores."},
            {"role": "user", "content": email_body},
        ]
    )
    return response.choices[0].message.content

# Context-Based Data Extraction for Banking
def extract_key_fields(email_body: str) -> Dict[str, Any]:
    response = client.chat.completions.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "Extract banking-related fields from the email, such as Loan Amount, Interest Rate, Credit Card Type, Transaction ID, and Expiration Date."},
            {"role": "user", "content": email_body},
        ]
    )
    return response.choices[0].message.content

# Main Execution
def process_email(email_text: str, attachment_path: str = None, rules: Dict[str, Any] = None) -> Dict[str, Any]:
    parsed_email = Parser().parsestr(email_text)
    email_body = parsed_email.get_payload()
    
    # Check for duplicate emails
    email_hash = get_email_hash(email_body)
    
    # Classify Email (Main & Sub-Request with Prioritization & Confidence Score)
    classification = classify_email(email_body)
    
    # Detect Intent & Prioritization
    primary_intent = detect_primary_intent(email_body)
    
    # Extract Fields
    extracted_fields = extract_key_fields(email_body)
    
    # Process Attachment (if any)
    attachment_text = extract_text_from_attachment(attachment_path) if attachment_path else None
    
    # Output structured result
    return {
        "email_hash": email_hash,
        "classification": classification,
        "primary_intent": primary_intent,
        "extracted_fields": extracted_fields,
        "attachment_text": attachment_text,
        "duplicate_flag": email_hash,  # Placeholder for duplicate check logic
        "duplicate_reason": "Potential duplicate detected based on email content hash" if email_hash else "Unique email"
    }

# Example Usage
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
