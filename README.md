# smartbook
# 🏥 Medical Appointment Automation — n8n

A fully automated appointment management system running over WhatsApp.  
Built with Google Sheets + Twilio + n8n.

---

## 📋 What It Does

1. **When an appointment is created** → customer receives a WhatsApp confirmation message
2. **1 day before the appointment** → "Are you coming tomorrow?" message is sent
3. **If the customer replies "No"** → the next person on the waiting list automatically receives an offer
4. **If they also decline** → moves to the next person, until the slot is filled

---

## 🗂️ Files

| File | Description |
|---|---|
| `workflow-1-randevu.json` | New appointment → confirmation message → reminder |
| `workflow-2-cevap.json` | Yes/No webhook → update Sheets → check waiting list |
| `workflow-3-bekleme.json` | Waiting list response → move to next person |

---

## 🛠️ Setup

### Requirements
- [n8n](https://n8n.io) (self-hosted or cloud)
- Google Sheets account
- Twilio account (WhatsApp Business API)

### Steps

**1. Prepare Google Sheets**

Create two sheets:

**Appointments sheet:**
| ID | Name | Phone | Date | Time | Status |
|---|---|---|---|---|---|
| 1 | John Doe | 1555... | 2026-03-20 | 14:00 | Pending |

**Waiting List sheet:**
| ID | Name | Phone | PreferredTime | Status |
|---|---|---|---|---|
| 1 | Jane Doe | 1555... | Afternoon | Waiting |

> ⚠️ Include country code in phone numbers, e.g. `15551234567`

**2. Import into n8n**

Import each JSON file one by one:
- n8n → top left menu `...` → `Import from file`
- Import all 3 workflows separately

**3. Update the variables**

In each workflow, replace the following:

```
SHEETS_DOCUMENT_ID     → The ID from your Google Sheets URL
                          docs.google.com/spreadsheets/d/[THIS_PART]/edit

TWILIO_WHATSAPP_NUMBER → Your Twilio WhatsApp number
                          e.g. +14155238886

TWILIO_CREDENTIAL_ID   → The credential ID after setting up Twilio in n8n

WEBHOOK_BASE_URL       → Your publicly accessible n8n address
                          e.g. https://n8n.repocloud.io
```

**4. Create Twilio credential in n8n**

- n8n → Settings → Credentials → New
- Select Twilio
- Enter your Account SID and Auth Token from the Twilio dashboard

**5. Activate the workflows**

Toggle all 3 workflows to **"Active"** in the top right corner.

---

## 💰 Cost

| Service | Cost |
|---|---|
| n8n (Repocloud) | Fixed monthly fee — number of executions doesn't matter |
| Google Sheets | Free |
| Twilio WhatsApp | ~$0.005/message — 50 messages/month ≈ $0.25 |

---

## 📞 Support

Feel free to open an issue in the Issues tab if you run into any problems.
