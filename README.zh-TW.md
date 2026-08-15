# 🤖 AI 客服自動化與評估系統

[🇺🇸 English](README.md) | 🇹🇼 繁體中文

這是一套使用 **n8n、OpenAI、Google Sheets 與 Gmail** 建立的 AI 客服自動化與評估系統。

這個專案不只讓 AI 自動處理客服訊息，也加入了一套自動化 AI 評估流程，可以自動產生測試案例、測試客服 AI 的分類結果，並計算系統準確率。

---

## 📌 專案介紹

本專案模擬實際的 AI 客服自動化系統。

當使用者送出客服訊息後，系統會自動：

- 判斷客服案件類型
- 判斷案件優先級
- 產生問題摘要
- 產生客服建議回覆
- 將客服紀錄儲存至 Google Sheets
- 遇到高優先級案件時，自動寄送 Gmail 通知

除了主要客服流程之外，我另外設計了一套 **AI Evaluation Workflow（AI 評估流程）**。

這套評估流程可以自動產生客服測試案例，將案例送入原本的客服系統，再比較 AI 預測結果與預期答案，最後計算分類準確率。

---

## ✨ 主要功能

### AI 客服自動化

- 透過 n8n Webhook 接收客服訊息
- 使用 OpenAI 分析客服內容
- 將客服案件分類為：
  - `inquiry`：一般詢問
  - `complaint`：客戶抱怨
  - `technical_issue`：技術問題
  - `other`：其他
- 判斷案件優先級：
  - `low`
  - `medium`
  - `high`
- 自動產生繁體中文問題摘要
- 自動產生繁體中文客服建議回覆
- 將客服紀錄寫入 Google Sheets
- 自動偵測高優先級案件
- 高優先級案件自動寄送 Gmail 通知

### AI 自動化評估系統

- 自動產生多筆客服測試案例
- 為測試案例建立預期 Category 與 Priority
- 透過 HTTP Request 將測試資料送入客服 Workflow
- 比較 Expected Label 與 AI Prediction
- 偵測無法解析的 AI JSON 輸出
- 計算 Category Accuracy
- 計算 Priority Accuracy
- 計算 Overall Accuracy
- 將測試結果自動儲存至 Google Sheets

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
```

這條 Workflow 負責實際處理客服案件。

使用者的訊息會先透過 Webhook 進入 n8n，再交由 OpenAI 分析案件類型與優先級，同時產生摘要與客服建議回覆。

分析結果會被寫入 Google Sheets。如果系統判斷案件屬於 `high priority`，則會進一步觸發 Gmail 通知。

---

### AI Evaluation Workflow

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

這條 Workflow 負責測試前面的客服 AI 是否能做出正確判斷。

系統會自動產生測試客服案件，並為每一筆測試資料建立 Expected Category 與 Expected Priority。

接著透過 HTTP Request 將測試案件送入真正的 Customer Support Workflow，再比較 AI 預測結果與預期結果。

---

## 🧪 AI 評估方式

目前評估兩個主要 AI 判斷：

1. **Category Classification**
2. **Priority Classification**

每一筆測試案例都會比較：

```text
Expected Category → Predicted Category
Expected Priority → Predicted Priority
```

最後自動計算：

```text
Category Accuracy
Priority Accuracy
Overall Accuracy
```

其中一次使用 5 筆自動產生測試案例的結果為：

| 評估指標 | 結果 |
|---|---:|
| Valid Cases | 5 / 5 |
| Category Accuracy | 80% |
| Priority Accuracy | 80% |
| Overall Accuracy | 80% |

---

## 🔍 評估中發現的問題

在測試過程中，我發現目前系統仍有可以改善的地方。

例如：

```text
客服問題：
訂單延遲，客戶詢問是否可以取消訂單並退款

Expected Priority：
medium

AI Prediction：
high
```

也就是說，AI 有時候會因為客服訊息中出現「退款」、「付款」等字詞，而傾向將案件判斷成 `high priority`。

這個測試結果讓我發現，單純讓 Workflow 成功執行並不足以證明 AI 系統的可靠性。

因此，我加入自動化 Evaluation Workflow，讓 AI 的分類結果可以被量化，並根據錯誤案例持續調整 Prompt 與 Priority Classification Rules。

---

## 🛠️ 使用技術

- **n8n** — Workflow Automation / 系統流程整合
- **OpenAI API** — 客服訊息分析與測試資料生成
- **JavaScript** — JSON Parsing 與 Evaluation Logic
- **HTTP / Webhook** — Workflow 之間的資料傳遞
- **Google Sheets API** — 客服紀錄與 Evaluation Results 儲存
- **Gmail API** — High Priority 自動通知
- **GitHub** — Workflow 版本管理與專案文件

---

## 📂 Repository 結構

```text
ai-customer-support-automation/
│
├── workflow/
│   ├── customer_support_workflow_public.json
│   └── evaluation_workflow_public.json
│
├── .gitignore
├── README.md
└── README.zh-TW.md
```

### Workflow Files

#### `customer_support_workflow_public.json`

主要的 AI 客服自動化 Workflow。

負責接收客服訊息、AI 分類、Priority 判斷、摘要與回覆生成、Google Sheets 紀錄以及 High Priority Gmail 通知。

#### `evaluation_workflow_public.json`

AI 自動化測試與評估 Workflow。

負責產生測試案例、呼叫 Customer Support Workflow、比較 Expected / Predicted Labels，並計算 AI 分類準確率。

---

## 💡 我從這個專案學到什麼

透過這個專案，我實際練習了：

- AI Workflow 設計
- LLM 與實際商業流程整合
- n8n 自動化流程設計
- Webhook 與 HTTP Request 串接
- OpenAI API 應用
- Structured JSON Output 處理
- JavaScript 資料解析
- AI Classification Rule 設計
- 自動化 AI Test Case Generation
- AI Classification Accuracy 評估
- LLM JSON Parse Error 處理
- Workflow Debugging
- Prompt Engineering
- Google Sheets / Gmail API 串接

在這個專案中，我認為最重要的學習是：

> **建立 AI 系統不只是讓 Workflow 成功執行，更重要的是確認 AI 做出的判斷是否正確，並建立可以持續評估與改善系統的方法。**

---

## 🚀 未來優化方向

目前系統仍有許多可以持續改善的地方：

- 調整 Priority Classification Rules
- 增加 Evaluation Dataset
- 加入更多 Edge Cases
- 建立 Confusion Matrix
- 比較不同 Prompt Version 的 Accuracy
- 比較不同 AI Model 的分類表現
- 建立 Automatic Regression Testing
- 建立 Evaluation Dashboard
- 分析 AI 常見 Misclassification Pattern

目前已透過 Evaluation Workflow 發現部分 `medium` 案件可能被判斷成 `high`，未來會進一步調整 Prompt 與分類規則，並透過相同 Evaluation Pipeline 驗證改善效果。

---

## 🔐 安全性

本 Repository 不包含：

- OpenAI API Key
- Google OAuth Credentials
- Gmail Credentials
- Google Sheets Credentials
- 其他敏感認證資訊

GitHub 中提供的 Workflow JSON 為移除敏感資訊後的公開展示版本。

---

## 👤 作者

**Jean Chuang**

Computer Science student interested in:

- AI Applications
- LLM Evaluation
- Workflow Automation
- System Integration
- AI Testing
