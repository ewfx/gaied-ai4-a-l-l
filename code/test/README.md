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
