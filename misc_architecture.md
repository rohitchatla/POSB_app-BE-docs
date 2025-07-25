# Miscellaneous, Architecture, and Diagrams

This document includes miscellaneous notes, architectural decisions, and diagrams for the POSB Account project.

## Contents
- Architectural Decisions
- Diagrams
- Miscellaneous Notes
- Automation & Scheduling
- Security Considerations

---

### Architectural Decisions
- Modular design: Separate scripts for business logic, web, Drive integration, and notifications
- Uses Google Drive for cloud storage and sharing
- Flask for web interface, pandas for data processing
- Logging to both terminal and file for traceability

### Diagrams
- Data Flow: [MIS/Source] → [Processing (pandas)] → [Report Generation] → [Google Drive Upload] → [Notification]
- Directory Structure: See `general.md` for details

### Miscellaneous Notes
- Credentials are stored in `service_account.json` and `personal_credentials.json`
- Uses `.env` and `settings.yaml` for configuration
- Reports are archived in dated folders for traceability

### Automation & Scheduling
- `automation.py` can be used for scheduled tasks (e.g., daily report generation)
- Can be integrated with cron or Windows Task Scheduler

### Security Considerations
- Sensitive credentials are not hardcoded; use environment variables or config files
- Only authorized users should have access to upload/download endpoints


<!-- rohitchatla: send excel files/images/any attachments with message to whatsapp using python to personal and group chats, give all ways using whatsapp business API, Add Twilio as well, summarize all ways to send whatsapp message with media in python -->

<!-- rohitchatla: send excel files/images/any attachments with message to whatsapp using python to personal and group chats, give all ways using whatsapp business API

GitHub Copilot: To send Excel files, images, or any attachments to WhatsApp (personal or group chats) using Python, you have several options. The **official WhatsApp Business API** is the most robust and scalable method, but it requires business approval and setup. Here’s a summary of all possible ways:

---

## 1. **Official WhatsApp Business API**

### **Prerequisites**
- WhatsApp Business Account
- Facebook Business verification
- WhatsApp Business API client/server running (self-hosted or via a provider)
- Phone number registered with WhatsApp Business API

### **Sending Media (Excel, Images, etc.)**

#### **Step 1: Upload Media**
First, upload your file to WhatsApp servers:

````python
import requests

url = "https://graph.facebook.com/v19.0/<PHONE_NUMBER_ID>/media"
headers = {
    "Authorization": "Bearer <ACCESS_TOKEN>"
}
files = {
    'file': open('your_file.xlsx', 'rb')
}
data = {
    "messaging_product": "whatsapp",
    "type": "document"
}
response = requests.post(url, headers=headers, files=files, data=data)
media_id = response.json()["id"]
````

#### **Step 2: Send Media Message**
Send the uploaded file to a user or group:

````python
import requests

url = "https://graph.facebook.com/v19.0/<PHONE_NUMBER_ID>/messages"
headers = {
    "Authorization": "Bearer <ACCESS_TOKEN>",
    "Content-Type": "application/json"
}
payload = {
    "messaging_product": "whatsapp",
    "to": "<RECIPIENT_PHONE_NUMBER>",  # For group, use group ID (see below)
    "type": "document",
    "document": {
        "id": media_id,
        "caption": "Here is your file"
    }
}
response = requests.post(url, headers=headers, json=payload)
````

- For **images**, use `"type": "image"` and `"image": {"id": media_id, ...}`.
- For **groups**, use the group’s WhatsApp ID as the `to` field (format: `group-id@whatsapp.net`).

#### **Notes**
- You cannot send messages to arbitrary numbers; users must have opted in.
- For groups, you need to be an admin and have the group’s WhatsApp ID.

---

## 2. **Third-Party Providers (Cloud APIs)**
Providers like Twilio, 360dialog, or Vonage offer managed WhatsApp Business API access. Their Python SDKs simplify sending media:

**Example with Twilio:**
````python
from twilio.rest import Client

client = Client("ACCOUNT_SID", "AUTH_TOKEN")
message = client.messages.create(
    from_='whatsapp:+14155238886',
    to='whatsapp:+<RECIPIENT_NUMBER>',
    media_url=['https://yourserver.com/your_file.xlsx']
)
````

---

## 3. **Unofficial Libraries (Not Recommended for Production)**
Libraries like `pywhatkit`, `selenium`, or `yowsup` can automate WhatsApp Web, but they are not officially supported and may break or violate WhatsApp’s terms.

---

## **Summary Table**

| Method                | Personal Chats | Group Chats | Attachments | Official/Supported |
|-----------------------|:-------------:|:-----------:|:-----------:|:------------------:|
| WhatsApp Business API |      Yes      |     Yes     |     Yes     |       Yes          |
| Twilio/360dialog      |      Yes      |     Yes     |     Yes     |       Yes          |
| pywhatkit/selenium    |      Yes      |     Yes     |     Yes     |        No          |

---

## **References**
- [WhatsApp Business API: Send Media](https://developers.facebook.com/docs/whatsapp/cloud-api/guides/send-media/)
- [Twilio WhatsApp API](https://www.twilio.com/docs/whatsapp/send-media-messages)

---

**Tip:** For production, always use the official API or a trusted provider. Unofficial methods are for personal/testing use only.

rohitchatla: Add Twilio as well

GitHub Copilot: ````markdown
### Sending Attachments to WhatsApp via Python

You can send Excel files, images, or other attachments to WhatsApp (personal or group chats) using Python in the following ways:

#### 1. **Official WhatsApp Business API**
- **Prerequisites:** WhatsApp Business Account, Facebook Business verification, API setup.
- **Steps:**
  1. **Upload Media:**
     ```python
     import requests

     url = "https://graph.facebook.com/v19.0/<PHONE_NUMBER_ID>/media"
     headers = {"Authorization": "Bearer <ACCESS_TOKEN>"}
     files = {'file': open('your_file.xlsx', 'rb')}
     data = {"messaging_product": "whatsapp", "type": "document"}
     response = requests.post(url, headers=headers, files=files, data=data)
     media_id = response.json()["id"]
     ```
  2. **Send Media Message:**
     ```python
     url = "https://graph.facebook.com/v19.0/<PHONE_NUMBER_ID>/messages"
     headers = {
         "Authorization": "Bearer <ACCESS_TOKEN>",
         "Content-Type": "application/json"
     }
     payload = {
         "messaging_product": "whatsapp",
         "to": "<RECIPIENT_PHONE_NUMBER>",  # For group, use group ID
         "type": "document",
         "document": {"id": media_id, "caption": "Here is your file"}
     }
     response = requests.post(url, headers=headers, json=payload)
     ```
  - For images, use `"type": "image"` and `"image": {"id": media_id, ...}`.
  - For groups, use the group’s WhatsApp ID as the `to` field.

#### 2. **Twilio WhatsApp API**
- **Prerequisites:** Twilio account, WhatsApp sender setup.
- **Steps:**
  ```python
  from twilio.rest import Client

  client = Client("ACCOUNT_SID", "AUTH_TOKEN")
  message = client.messages.create(
      from_='whatsapp:+14155238886',
      to='whatsapp:+<RECIPIENT_NUMBER>',
      media_url=['https://yourserver.com/your_file.xlsx']
  )
  ```
- Supports sending to both personal and group chats (group via group ID).

#### 3. **Unofficial Libraries (Not Recommended for Production)**
- Libraries like `pywhatkit`, `selenium`, or `yowsup` can automate WhatsApp Web to send attachments, but are not officially supported and may break or violate WhatsApp’s terms.

**References:**  
- [WhatsApp Business API: Send Media](https://developers.facebook.com/docs/whatsapp/cloud-api/guides/send-media/)  
- [Twilio WhatsApp API](https://www.twilio.com/docs/whatsapp/send-media-messages)
````

rohitchatla: summarize all ways to send whatsapp message with media in python

GitHub Copilot: Here’s a summary of all ways to send WhatsApp messages with media (Excel, images, attachments) using Python:

---

### 1. **Official WhatsApp Business API**
- **Requirements:** WhatsApp Business Account, Facebook Business verification, API setup.
- **How it works:**
  1. **Upload media** to WhatsApp servers using a POST request.
  2. **Send message** referencing the uploaded media ID to a user or group.
- **Supports:** Personal and group chats, all media types.
- **Docs:** [WhatsApp Business API: Send Media](https://developers.facebook.com/docs/whatsapp/cloud-api/guides/send-media/)

---

### 2. **Third-Party Providers (e.g., Twilio)**
- **Requirements:** Account with provider (e.g., Twilio), WhatsApp sender setup.
- **How it works:** Use provider’s Python SDK to send messages with media URLs.
- **Supports:** Personal and group chats, all media types.
- **Example:**  
    ```python
    from twilio.rest import Client
    client = Client("ACCOUNT_SID", "AUTH_TOKEN")
    message = client.messages.create(
        from_='whatsapp:+14155238886',
        to='whatsapp:+<RECIPIENT_NUMBER>',
        media_url=['https://yourserver.com/your_file.xlsx']
    )
    ```
- **Docs:** [Twilio WhatsApp API](https://www.twilio.com/docs/whatsapp/send-media-messages)

---

### 3. **Unofficial Libraries (Not Recommended for Production)**
- **Examples:** `pywhatkit`, `selenium`, `yowsup`
- **How it works:** Automate WhatsApp Web to send messages and attachments.
- **Risks:** Not officially supported, may break or violate WhatsApp’s terms.

---

**Summary Table**

| Method                | Personal Chats | Group Chats | Attachments | Official/Supported |
|-----------------------|:-------------:|:-----------:|:-----------:|:------------------:|
| WhatsApp Business API |      Yes      |     Yes     |     Yes     |       Yes          |
| Twilio/Providers      |      Yes      |     Yes     |     Yes     |       Yes          |
| pywhatkit/selenium    |      Yes      |     Yes     |     Yes     |        No          |

**Tip:** For production, use the official API or a trusted provider. Unofficial methods are for testing only. -->