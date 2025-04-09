---
layout: single
title: "Smart Email Routing"
date: 2025-01-18 09:30:45 +0530
categories:
  - full-stack
tags:
  - node js
  - security headers
  - web app
  - helmet
  - production
toc: true
classes: wide
---

## Introduction

In the age of overflowing inboxes, email triaging can become a real pain—especially when you're part of a fast-moving organization. Manually sorting and forwarding emails can waste hours every week. I decided to automate this task by building a **Smart Email Routing System** using the **Gmail API** and the **LLaMA 3.2 model from Ollama**.

The goal?  
Build a GenAI-powered bot that can:
- Read incoming emails.
- Understand their context using LLaMA.
- Decide **who** the email should go to.
- Determine **how urgent** it is.
- Route it intelligently—no human involved.

Let's walk through how this system works.

## How It Works

The architecture is pretty straightforward, but very powerful. Here's the breakdown:

### 📨 1. Fetch Emails using Gmail API
Using OAuth 2.0 and the `google-api-python-client`, the system:
- Authenticates securely using a `token.json`.
- Connects to the Gmail inbox.
- Fetches all **unread** emails from the `INBOX`.
- Extracts the subject, sender, and message body (MIME decoded).

### 🧠 2. Pass Email to LLaMA 3.2 via Ollama
Once the email is fetched:
- We combine the subject + body into a prompt.
- Send it to a **locally running LLaMA 3.2 model via Ollama CLI**.
- The prompt asks LLaMA to return:
  - `To whom should this email be routed?`
  - `What is the severity level?` (Low / Medium / High / Urgent)

### 🧾 3. Parse the Response
The model returns a JSON-like output such as:

```json
{
  "route_to": "Legal Team",
  "severity": "High"
}
```

This output is parsed and used in the next step.

### 🏷️ 4. Route and Tag the Email
Based on the model's output:
- Apply appropriate Gmail labels (e.g., `to_legal`, `severity_high`)
- Optionally forward the email to the correct department
- Archive or move it as needed

## Implementation Code

Here's the full implementation for this project:

```python
import os
import base64
import json
import email
from email.mime.text import MIMEText
from googleapiclient.discovery import build
from google_auth_oauthlib.flow import InstalledAppFlow
from google.auth.transport.requests import Request
from google.oauth2.credentials import Credentials
import subprocess

# Define the scopes needed for Gmail API
SCOPES = ['https://www.googleapis.com/auth/gmail.modify']

def authenticate_gmail():
    """
    Authenticate with Gmail API using OAuth2
    """
    creds = None
    if os.path.exists('token.json'):
        creds = Credentials.from_authorized_user_info(json.load(open('token.json')))
    
    if not creds or not creds.valid:
        if creds and creds.expired and creds.refresh_token:
            creds.refresh(Request())
        else:
            flow = InstalledAppFlow.from_client_secrets_file(
                'credentials.json', SCOPES)
            creds = flow.run_local_server(port=0)
        
        with open('token.json', 'w') as token:
            token.write(creds.to_json())
    
    return build('gmail', 'v1', credentials=creds)

def get_email_content(service, msg_id):
    """
    Extract email content from message ID
    """
    message = service.users().messages().get(userId='me', id=msg_id, format='full').execute()
    
    headers = message['payload']['headers']
    subject = next((header['value'] for header in headers if header['name'].lower() == 'subject'), '(No Subject)')
    sender = next((header['value'] for header in headers if header['name'].lower() == 'from'), 'Unknown')
    
    parts = [message['payload']]
    body = ""
    
    while parts:
        part = parts.pop(0)
        
        if 'parts' in part:
            parts.extend(part['parts'])
        
        if 'body' in part and 'data' in part['body']:
            body_bytes = base64.urlsafe_b64decode(part['body']['data'])
            body += body_bytes.decode('utf-8')
    
    return {
        'subject': subject,
        'sender': sender,
        'body': body,
        'id': msg_id
    }

def analyze_with_llama(subject, body):
    """
    Send email content to LLaMA 3.2 via Ollama CLI for analysis
    """
    prompt = f"""You are a smart email assistant.

Classify the following email and respond in JSON format with:
1. Who it should be routed to.
2. Severity level: Low, Medium, High, or Urgent.

Email:
Subject: {subject}
Body: {body}
"""

    # Execute Ollama command - adjust model name as needed
    result = subprocess.run(
        ["ollama", "run", "llama3.2", prompt],
        capture_output=True,
        text=True
    )
    
    # Extract JSON from response
    try:
        # Find JSON-like content in the output
        response = result.stdout.strip()
        start_idx = response.find('{')
        end_idx = response.rfind('}') + 1
        
        if start_idx >= 0 and end_idx > start_idx:
            json_str = response[start_idx:end_idx]
            return json.loads(json_str)
        else:
            return {"route_to": "Unknown", "severity": "Medium"}
    except Exception as e:
        print(f"Error parsing LLaMA response: {e}")
        return {"route_to": "Unknown", "severity": "Medium"}

def apply_routing(service, email_data, routing_info):
    """
    Apply routing decisions to email (labels, forwarding, etc.)
    """
    msg_id = email_data['id']
    route_to = routing_info.get('route_to', 'Unknown').lower().replace(' ', '_')
    severity = routing_info.get('severity', 'Medium').lower()
    
    # Create labels if they don't exist
    labels_to_apply = [f"to_{route_to}", f"severity_{severity}"]
    existing_labels = service.users().labels().list(userId='me').execute().get('labels', [])
    existing_label_names = [label['name'] for label in existing_labels]
    
    for label_name in labels_to_apply:
        if label_name not in existing_label_names:
            label_object = {'name': label_name}
            service.users().labels().create(userId='me', body=label_object).execute()
    
    # Get label IDs for application
    updated_labels = service.users().labels().list(userId='me').execute().get('labels', [])
    label_ids = [label['id'] for label in updated_labels if label['name'] in labels_to_apply]
    
    # Apply labels
    service.users().messages().modify(
        userId='me',
        id=msg_id,
        body={'addLabelIds': label_ids, 'removeLabelIds': ['UNREAD']}
    ).execute()
    
    # Optional: Forward email based on routing
    # This part would connect to your forwarding logic
    
    return {
        'applied_labels': labels_to_apply,
        'route_to': routing_info.get('route_to'),
        'severity': routing_info.get('severity')
    }

def main():
    """
    Main function to orchestrate email processing
    """
    # Authenticate
    service = authenticate_gmail()
    
    # Get unread emails
    results = service.users().messages().list(
        userId='me', 
        q='is:unread in:inbox'
    ).execute()
    
    messages = results.get('messages', [])
    
    if not messages:
        print("No unread messages found.")
        return
    
    print(f"Found {len(messages)} unread emails. Processing...")
    
    for message in messages:
        # Get email content
        email_data = get_email_content(service, message['id'])
        
        # Analyze with LLaMA
        routing_info = analyze_with_llama(email_data['subject'], email_data['body'])
        
        # Apply routing decisions
        result = apply_routing(service, email_data, routing_info)
        
        print(f"Processed email: '{email_data['subject']}'")
        print(f"  → Routed to: {result['route_to']}")
        print(f"  → Severity: {result['severity']}")
        print(f"  → Labels: {', '.join(result['applied_labels'])}")
        print("-" * 50)

if __name__ == "__main__":
    main()
```

## Sample Prompt Sent to LLaMA

```
You are a smart email assistant.

Classify the following email and respond in JSON format with:
1. Who it should be routed to.
2. Severity level: Low, Medium, High, or Urgent.

Email:
Subject: {{subject}}
Body: {{email_body}}
```

This zero-shot prompt approach worked surprisingly well!

## Why Not Just Rule-Based Filters?

Rule-based filters (like if subject contains "invoice" then forward) are brittle. They:

- Miss context.
- Fail on misspellings or vague language.
- Don't scale with varied email types.

Using LLaMA lets us handle:

- Semantics (not just keywords)
- Complex business logic
- Adaptable rules based on intent

## Final Thoughts

This project showed me how GenAI can elegantly solve boring but critical workflows. With just:

- A Gmail API connection,
- A local LLaMA model (via Ollama),
- And some smart prompting…

…I built a system that's capable of understanding and organizing email like a human assistant—but faster, and without ever needing coffee ☕.

Next steps?
- 🔁 Add scheduling
- 📊 Log and visualize routing stats
- 📥 Maybe even reply to some emails automatically...