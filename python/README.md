# email-intel (Python)

Enterprise standard email intelligence and verification library for Python applications and CLI.

This library is primarily focused on **checking if an email is valid**, **checking if it's a temp/disposable email**, and inferring the underlying email provider (e.g. Google Workspace, Microsoft 365, Zoho) with deep DNS analysis.

## Features

- **Email Validation**: Real-time MX record resolution to check if the domain can actually receive emails.
- **Disposable Email Detection**: Checks domains against an automatically updated daily list of thousands of disposable/temp email services (like 10minutemail, GuerrillaMail).
- **Provider Inference**: Identifies enterprise security gateways and providers like Proofpoint, Mimecast, Google Workspace, Microsoft 365, Zoho, etc.
- **Domain Classification**: Intelligently classifies domains into `Education`, `Government`, `Organization`, `Public Webmail`, or `Business` based on TLDs and regex.
- **Synchronous Execution**: Utilizes native `dnspython` for extremely robust DNS probing.

## Installation

```bash
pip install email-intel
```

---

## Integration Guide

### Backend Integration (Python)

In your Python backend (e.g., FastAPI, Django, Flask), you can use `email-intel` to block signups from disposable emails or validate that an email's domain actually exists before saving it to your database.

Here is an example using **FastAPI**:

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from email_intel import analyze

app = FastAPI()

class RegisterRequest(BaseModel):
    email: str

@app.post("/register")
async def register(req: RegisterRequest):
    try:
        report = analyze(req.email)
    except Exception as e:
        raise HTTPException(status_code=500, detail="Email validation failed")

    # 1. Check if the email domain is valid (has MX records)
    if not report.get("valid"):
        raise HTTPException(status_code=400, detail="Invalid email domain. Please provide a real email.")

    # 2. Check if the email is a disposable/temp email
    if report.get("disposable"):
        raise HTTPException(status_code=400, detail="Disposable emails are not allowed.")

    # 3. Optional: Block free public webmails if you only want B2B users
    if report.get("type") == "Public Webmail":
        raise HTTPException(status_code=400, detail="Please use your company email address.")

    # Proceed with registration...
    return {"message": "Registration successful!", "data": report}
```

---

## CLI Usage

You can also use the package as a CLI tool in your terminal:

```bash
email-intel shivam@test.com
```

This will output a nice, formatted intelligence report utilizing `rich`!
