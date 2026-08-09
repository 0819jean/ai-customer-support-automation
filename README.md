# 🤖 AI Customer Support Automation

An AI-powered customer support automation workflow built with n8n, OpenAI, Google Sheets, and Gmail.

## 📌 Project Overview

This project is an AI-powered customer support automation system built with n8n. It automatically analyzes incoming customer messages, identifies the request category and priority, generates a summary and suggested reply, and stores the results in Google Sheets.

For high-priority customer requests, the workflow automatically sends a Gmail alert so that urgent issues can be handled quickly.

## ✨ Features

- Receive customer requests through an n8n Webhook
- Analyze customer messages using AI
- Classify requests by category and priority
- Generate an AI summary and suggested reply
- Store customer support records in Google Sheets
- Automatically detect high-priority cases
- Send Gmail alerts for high-priority customer requests

## 🔄 Workflow Architecture

Customer Request  
↓  
AI Customer Analysis  
↓  
Parse AI Response  
↓  
Save Support Record  
↓  
Check Priority  
↓  
High Priority → Send Gmail Alert

## 🛠️ Tech Stack

| Technology | Purpose |
| --- | --- |
| n8n | Workflow automation and orchestration |
| OpenAI GPT-5-mini | Customer message analysis and response generation |
| JavaScript | Parse AI responses and transform data |
| Google Sheets | Store customer support records |
| Gmail | Send alerts for high-priority cases |
| Webhook / HTTP POST | Receive customer requests |

## 📦 AI Structured Output

The AI model analyzes each customer message and returns structured JSON data:

```json
{
  "category": "complaint",
  "priority": "high",
  "summary": "收到的商品損壞，且客服尚未回覆，需要盡快處理。",
  "suggested_reply": "您好，非常抱歉造成您的困擾。我們已收到您的問題，將協助您進一步處理。"
}
```

The JavaScript node parses the AI response and combines it with the original customer information for use by the following workflow nodes.

## 📸 Demo Screenshots

### n8n Workflow

The complete automation workflow built with n8n.
<img width="864" height="424" alt="image" src="https://github.com/user-attachments/assets/e10671d7-a485-4bb0-a086-dda77ad34f80" />


### High Priority Email Alert

When the AI identifies a customer request as high priority, the workflow automatically sends an email alert through Gmail.
<img width="841" height="461" alt="image" src="https://github.com/user-attachments/assets/38a3801f-85fc-4be4-a389-6389341c3247" />

### Google Sheets Support Records

All customer requests and AI analysis results are automatically stored in Google Sheets, including the request category, priority, summary, and suggested reply.
<img width="1100" height="659" alt="螢幕擷取畫面 2026-08-09 132704" src="https://github.com/user-attachments/assets/904521bd-e998-45bb-a825-0d7ef9960b4f" />



---
## ⚙️ How It Works

1. **Customer Request**
   - Receives customer information through an n8n Webhook using an HTTP POST request.

2. **AI Customer Analysis**
   - Sends the customer message to the OpenAI model.
   - The AI analyzes the request and generates `category`, `priority`, `summary`, and `suggested_reply`.

3. **Parse AI Response**
   - Uses JavaScript to parse the AI-generated JSON response.
   - Combines the AI analysis with the original customer information.

4. **Save Support Record**
   - Stores the customer information and AI analysis results in Google Sheets.

5. **Check Priority**
   - Checks whether the customer request is classified as `high` priority.

6. **Send High Priority Alert**
   - If the priority is `high`, the workflow automatically sends a Gmail alert.

## 🧪 Example Test Case

### Customer Request

```json
{
  "name": "王小美",
  "email": "xiaomei@example.com",
  "message": "我收到的商品是損壞的，而且已經聯絡客服三次都沒有收到任何回覆。我非常不滿意，因為這個商品是急著要使用的，希望你們可以立即處理並提供換貨或退款。"
}
```
### AI Analysis Result

The AI analyzes the customer request and returns structured data:

```json
{
  "category": "complaint",
  "priority": "high",
  "summary": "收到的商品為損壞品，已三次聯絡客服但未獲回覆，因急需使用，要求立即換貨或退款。",
  "suggested_reply": "非常抱歉造成您的困擾與不便，我們已收到您的問題，將優先協助您處理。"
}
```

### Automation Result

Because the request is classified as `high` priority:

- The customer support record is automatically stored in Google Sheets.
- The workflow follows the `TRUE` branch of the priority check.
- A high-priority alert email is automatically sent through Gmail.

---

## 🚀 Future Improvements

- Automatically send AI-generated replies to customers
- Add more detailed priority levels and routing rules
- Integrate LINE Bot or other messaging platforms
- Store customer history in a database
- Build a dashboard for monitoring customer support cases
- Add human approval before sending AI-generated responses

## 📂 Project Structure

```text
ai-customer-support-automation/
│
├── README.md
│
├── workflow/
│   └── ai-customer-support-workflow-public.json
│
└── screenshots/
    ├── workflow.png
    └── high-priority-email-alert.png
```
