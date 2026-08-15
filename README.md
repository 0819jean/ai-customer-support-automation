# 🤖 AI Customer Support Automation & Evaluation System

[🇺🇸 English](README.md) | [🇹🇼 繁體中文](README.zh-TW.md)

An AI-powered customer support automation and evaluation system built with **n8n, OpenAI, Google Sheets, and Gmail**.

This project not only automates customer support workflows, but also includes an automated evaluation pipeline that generates test cases, evaluates AI classification results, and measures system accuracy.

---

## 📌 Project Overview

This project simulates an AI-powered customer support system that automatically processes incoming customer messages.

The system can:

- Classify customer requests into different categories
- Determine request priority
- Generate a concise issue summary
- Generate a suggested customer service reply
- Store support records in Google Sheets
- Automatically send Gmail alerts for high-priority cases

To evaluate the reliability of the AI classification system, I also built a separate **AI Evaluation Workflow**.

The evaluation workflow automatically generates customer support test cases, sends them through the customer support system, compares the AI predictions with expected labels, and calculates classification accuracy.

---

## ✨ Main Features

### Customer Support Automation

- Receive customer requests through an n8n Webhook
- Analyze customer messages using OpenAI
- Classify requests into:
  - `inquiry`
  - `complaint`
  - `technical_issue`
  - `other`
- Assign priority:
  - `low`
  - `medium`
  - `high`
- Generate Traditional Chinese summaries
- Generate suggested customer service replies
- Store results in Google Sheets
- Detect high-priority cases
- Send Gmail alerts automatically

### AI Evaluation System

- Automatically generate multiple customer support test cases
- Generate expected category and priority labels
- Send test cases to the customer support workflow through HTTP requests
- Compare expected labels with AI predictions
- Detect invalid or unparsable outputs
- Calculate category accuracy
- Calculate priority accuracy
- Calculate overall classification accuracy
- Store evaluation results in Google Sheets

---

## 🔄 System Architecture

### Customer Support Workflow

```text
Customer Request
      ↓
Webhook
      ↓
OpenAI Customer Analysis
      ↓
Parse AI Response
      ↓
Google Sheets
      ↓
Priority Check
      ↓
High Priority?
      ↓
Gmail Alert
```

### Evaluation Workflow

```text
Manual Trigger
      ↓
Generate Test Runs
      ↓
Generate Customer Cases
      ↓
HTTP Request
      ↓
Customer Support Workflow
      ↓
Evaluate Results
      ↓
Google Sheets Evaluation Summary
```

---

## 🧪 AI Evaluation

The evaluation workflow compares two AI predictions:

1. **Category Classification**
2. **Priority Classification**

For each test case, the system records:

```text
Expected Category → Predicted Category
Expected Priority → Predicted Priority
```

It then calculates:

```text
Category Accuracy
Priority Accuracy
Overall Accuracy
```

During one evaluation run with 5 automatically generated test cases, the system achieved:

| Metric | Result |
|---|---:|
| Valid Cases | 5 / 5 |
| Category Accuracy | 80% |
| Priority Accuracy | 80% |
| Overall Accuracy | 80% |

The evaluation also revealed an important issue: some medium-priority customer complaints were classified as high priority.

Instead of manually inspecting individual cases only, this evaluation pipeline makes the classification behavior measurable and provides a clear direction for future prompt optimization.

---

## 🔍 Example Evaluation Finding

Example:

```text
Customer Issue:
Delayed order with a request for cancellation/refund

Expected Priority:
medium

AI Prediction:
high
```

This demonstrates that the current model may sometimes interpret payment-related or refund-related language as more urgent than expected.

This is a useful finding for improving priority classification rules and prompt design.

---

## 🛠️ Technologies

- **n8n** — workflow automation and orchestration
- **OpenAI API** — customer message analysis and test case generation
- **JavaScript** — JSON parsing and evaluation logic
- **HTTP / Webhook** — communication between workflows
- **Google Sheets API** — support records and evaluation results
- **Gmail API** — high-priority notifications
- **GitHub** — workflow version control and project documentation

---

## 📂 Repository Structure

```text
ai-customer-support-automation/
│
├── workflow/
│   ├── customer_support_workflow_public.json
│   └── evaluation_workflow_public.json
│
├── .gitignore
└── README.md
```

### Workflow Files

`customer_support_workflow_public.json`

Main AI customer support automation workflow.

`evaluation_workflow_public.json`

Automated AI testing and evaluation workflow.

---

## 💡 What I Learned

Through this project, I gained hands-on experience with:

- Designing AI-powered automation workflows
- Integrating LLMs into real business processes
- Building APIs with Webhooks and HTTP requests
- Handling structured JSON responses from LLMs
- Designing AI classification rules
- Creating automated AI test cases
- Measuring AI classification accuracy
- Debugging malformed AI-generated JSON
- Evaluating model behavior instead of relying only on successful demos
- Identifying classification errors and planning prompt improvements

One of the most important lessons from this project is that **building an AI system is not only about making the workflow work — it is also important to evaluate whether the AI is making the correct decisions.**

---

## 🚀 Future Improvements

Future improvements may include:

- Refine priority classification rules
- Expand the evaluation dataset
- Test more edge cases
- Add confusion matrix analysis
- Track accuracy across different prompt versions
- Compare different AI models
- Add automatic regression testing
- Build a dashboard for evaluation metrics

---

## 🔐 Security

API keys, OAuth credentials, and other sensitive information are not included in this repository.

The workflow files are provided as public versions for demonstration purposes.

---

## 👤 Author

**Jean Chuang**

Computer Science student interested in AI applications, workflow automation, system integration, and LLM evaluation.

---

# 🇹🇼 繁體中文版

## 📌 專案介紹

這是一套使用 **n8n、OpenAI、Google Sheets 與 Gmail** 建立的 AI 客服自動化與評估系統。

這個專案不只讓 AI 自動處理客服訊息，我也另外建立了一套 **AI Evaluation Workflow**，用來自動產生測試案例、驗證 AI 的分類結果，並計算系統的分類準確率。

整個專案主要分成兩個部分：

### 1. AI 客服自動化系統

當客戶送出客服訊息後，系統會自動：

- 分析客戶訊息
- 判斷問題類型：
  - `inquiry`
  - `complaint`
  - `technical_issue`
  - `other`
- 判斷處理優先級：
  - `low`
  - `medium`
  - `high`
- 產生繁體中文問題摘要
- 產生建議客服回覆
- 將客服紀錄寫入 Google Sheets
- 偵測高優先級案件
- 高優先級案件自動寄送 Gmail 通知

### 2. AI 自動評估系統

為了確認 AI 的分類結果是否可靠，我另外建立了一套自動化 Evaluation Workflow。

系統會：

1. 自動產生多筆模擬客服案件
2. 為每筆案例設定 Expected Category 與 Expected Priority
3. 透過 HTTP Request 將案例送入原本的客服 Workflow
4. 取得 AI 的 Predicted Category 與 Predicted Priority
5. 自動比較 Expected 與 Predicted 結果
6. 偵測無法解析或格式錯誤的 AI 輸出
7. 計算 Category、Priority 與 Overall Accuracy
8. 將評估結果寫入 Google Sheets

---

## 🔄 系統架構

### Customer Support Workflow

```text
Customer Request
      ↓
Webhook
      ↓
OpenAI Customer Analysis
      ↓
Parse AI Response
      ↓
Google Sheets
      ↓
Priority Check
      ↓
High Priority?
      ↓
Gmail Alert
