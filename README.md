# 🎂 Birthday Wish to Email AI Agent

### 🛠️ **How it currently works**

*   🚀 **Trigger:** You manually start the workflow by clicking 'Execute'.
*   📊 **Fetch Data:** It pulls your birthday list from a specific **Google Sheet**.
*   🔍 **Filter:** It checks if the first 5 characters of the date in your sheet match **today’s date** (`dd-MM`).
*   🔄 **Loop:** It processes every person celebrating a birthday today, one by one.
*   🤖 **AI Generation:** It uses **Claude Opus 4.7** to craft a fresh, unique subject line and email message for every recipient.
*   📧 **Send:** It delivers the final message through **Gmail**, automatically pairing the recipient's name with the AI's creativity.

<img width="923" height="201" alt="image" src="https://github.com/user-attachments/assets/f857e948-d2c2-47c9-a59c-802e7039cbb1" />

# 🎂 Birthday Wish to WhatsApp AI Agent

### 🛠️ **How it currently works**

*   🚀 **Trigger:** You manually start the workflow by clicking 'Execute'.
*   📊 **Fetch Data:** It pulls your birthday list from a specific **Google Sheet**.
*   🔍 **Filter:** It checks if the first 5 characters of the date in your sheet match **today’s date** (`dd-MM`).
*   🔄 **Loop:** It processes every person celebrating a birthday today, one by one.
*   🤖 **AI Generation:** It uses **Claude Opus 4.7** to craft a fresh, unique email message for every recipient.
*   📧 **Send:** It delivers the final message through **WhatsApp**, automatically pairing the recipient's name with the AI's creativity.

<img width="928" height="176" alt="image" src="https://github.com/user-attachments/assets/6156eded-24cd-46bc-8fd8-7a69bbb2d6bb" />

# 🎓 Student Management AI Agent

### 🛠️ **How it works**

1.  📩 **The Trigger (Gmail):** 
    The workflow polls your Gmail account every minute for **unread emails**. When a new email arrives, it captures the sender, subject, and the body of the message.
2.  🧠 **The AI Brain (Claude 3.5 Sonnet):**
    The email data is passed to an **AI Agent** powered by Anthropic's Claude model. This agent is programmed with specific "System Instructions" to act as a record manager. It has **Simple Memory**, meaning it can remember the context of a conversation within the same email thread.
3.  🛠️ **The Tools (Google Sheets):**
    The AI Agent doesn't just "think"—it has "hands." It is connected to three specific Google Sheets tools:
    *   **Get Rows:** To check if a student already exists (using their Roll Number).
    *   **Append Row:** To add a new student's details.
    *   **Delete Row:** To remove a student's record.
4.  📝 **The Processing Logic:**
    *   **"add student" subject:** The AI parses the Roll No, Name, Age, and Branch. It checks if the Roll No exists. If it's a duplicate, it refuses; if it's unique, it adds them to the sheet.
    *   **"delete student" subject:** The AI finds the student by Roll No and removes them.
    *   **Invalid subject:** It politely tells the sender that the request couldn't be processed.
5.  📧 **The Output (Gmail Reply):**
    Once the AI completes the task (or hits an error), it generates a professional response and sends it back as a **Gmail Reply** to the original sender.

<img width="894" height="305" alt="image" src="https://github.com/user-attachments/assets/792d7780-9fc8-4406-9f75-67af01c4e84d" />

# 📑 Zabbix Host List Exporter

### 🛠️ **How it works**

1.  🏁 **Trigger:** The workflow is started manually by clicking the **"Execute workflow"** button.
2.  📡 **API Fetch (List Zabbix Hosts):** It makes a `POST` request to your Zabbix API using JSON-RPC. It specifically asks for a list of all hosts currently configured in your system.
3.  ✂️ **Data Extraction (Hosts Split):** The API response usually comes back as one large object. This node "splits" that data so every individual host becomes its own separate item.
4.  ✏️ **Formatting (Update Column Names):** This node renames the raw technical fields from Zabbix to make them human-readable:
    *   `host` becomes **Host Name**
    *   `hostid` becomes **Host ID**
5.  📊 **File Creation:** it takes that cleaned list and converts it into a **.xlsx (Excel)** file named "Zabbix Hosts.xlsx."
6.  ✉️ **Email Delivery:** Finally, it sends an email to a specific admin address with the **Excel file attached**.

<img width="1043" height="151" alt="image" src="https://github.com/user-attachments/assets/f556e6cd-fcb9-49be-8b5b-a680b7d10946" />

# 🛰️ Zabbix Monitoring Management

### 🛠️ **How it works**

1.  🏁 **Entry Point (Form Trigger):** 
    The workflow begins with a **Zabbix - Add / List Monitors** form. Users choose an action from a dropdown menu, such as adding a **URL**, **Port**, or **Database** (MySQL, MSSQL, Postgres, Oracle) monitor, or listing current monitors.
2.  ⚖️ **The Switch:** 
    A **Switch Node** routes the request to one of **eight specific paths** based on the user's selection in the first step.
3.  📝 **Data Collection & File Handling:** 
    *   **Sub-Forms:** For "Add" actions, a secondary form asks for details like *Application Name*, *ASK ID*, and an Excel (`.xlsx`) template file containing the monitor configurations.
    *   **Local Storage:** The workflow saves these uploaded Excel files to a temporary disk location (`/tmp/`) to ensure the data is ready for processing.
4.  🛡️ **Confirmation Gate:** 
    Before changes are made, a **Switch Node** ("Create Monitors?") checks if the user selected **"Yes"** on the confirmation radio button. This prevents accidental executions.
5.  🚀 **Execution (Sub-workflows):** 
    If confirmed, the workflow triggers a specific **Sub-workflow** (e.g., *Add URL Monitors*) to handle the technical heavy lifting of interacting with the Zabbix API.
6.  📋 **Listing Monitors:** 
    For "List" actions, the workflow skips file saving and triggers sub-workflows that retrieve data from Zabbix and likely email a report back to the user.

<img width="372" height="402" alt="image" src="https://github.com/user-attachments/assets/fc63122f-2ea7-4e21-bed1-f0cb8c2a582f" />

# 🛰️ Zabbix - List WEB Monitors

### 🛠️ **How it works**

1.  🏁 **Trigger (Parent Workflow):** 
    This workflow is called by a "Parent" process that passes in an **Application Name** and an **Email ID**.
2.  🔡 **Name Variation Generator:** 
    Using a custom JavaScript node, it creates every possible case combination (UPPER, lower, Title Case, etc.) for the application name. This ensures that even if Zabbix has inconsistent naming (e.g., "App-One" vs "APP-ONE"), the workflow will find it.
3.  📡 **Multi-Stage Zabbix API Fetch:**
    *   **Host Get:** Searches Zabbix for hosts tagged with those application name variations.
    *   **HTTPTESTs Get:** For every host found, it retrieves all configured **Web Scenarios** (monitors), including steps, URLs, and retry settings.
    *   **Items & Triggers Get:** It goes even deeper to find the current **Last Value** (status) and the specific **Trigger Expressions** (the "alert rules") linked to those web monitors.
4.  📝 **Deep Data Formatting:** 
    The **Update Column Name** node contains complex logic to transform raw Zabbix JSON into a readable summary. It extracts:
    *   **Web Steps:** Details on every URL step (Timeout, Status Codes, POST data).
    *   **Triggers:** The severity and status of the alerts.
    *   **Tags:** Any metadata attached to the monitor.
5.  📊 **Excel Generation:** 
    All this data is flattened into a professional `.xlsx` file. The filename is dynamically set to the `Application Name`.
6.  ✉️ **Email Delivery:** 
    The Excel report is emailed to the requester and the Zabbix admin simultaneously.

<img width="997" height="148" alt="image" src="https://github.com/user-attachments/assets/0d49569a-c579-4ec8-92a3-f97bac6ec661" />

# 🛰️ Zabbix - List Non-WEB Monitors

### 🛠️ **How it works**

1.  🏁 **The Trigger (Parent Workflow):**
    The workflow is initiated by a central parent process, receiving an **Application Name** and a recipient **Email ID** as input.
2.  🔡 **Smart Name Matching:**
    It uses a JavaScript **Code Node** to generate every possible variation of the application name (UPPERCASE, lowercase, Title Case, etc.). This ensures the workflow finds the monitoring data even if it was tagged inconsistently in Zabbix.
3.  📡 **Deep Zabbix Data Mining:**
    *   **Host Get:** Queries the Zabbix API to find all hosts tagged with the application's name.
    *   **Items Get:** For those hosts, it pulls every standard monitor (Zabbix agent, SNMP, SSH, Database monitors, etc.).
    *   **Trigger Expression:** It fetches the "Alert Rules" (Triggers) connected to those items to see exactly what thresholds will cause a notification.
4.  📝 **Human-Friendly Formatting:**
    The **Update Column Name** node translates raw technical data into readable text. It maps Zabbix numeric codes to clear labels, such as converting `Type: 0` into **"Zabbix agent"** and `Status: 0` into **"Enabled."**
5.  📊 **File Conversion & Delivery:**
    *   **Excel:** Bundles host names, monitor types, keys, and last recorded values into a professional `.xlsx` file.
    *   **Email:** The final report is automatically sent to the provided email address, providing a full inventory of the non-web monitoring status.
  
<img width="1001" height="135" alt="image" src="https://github.com/user-attachments/assets/b45bfd7b-14af-4db4-8af8-0067eb597c1e" />

## n8n Workflow: Latest AI News to Email
This workflow automates the process of gathering AI news from multiple sources, summarizing it using Claude Opus (Anthropic), and delivering a report via Gmail.
## ⚙️ Workflow Components## 1. Automation Trigger
* Node: Schedule Trigger
* Frequency: Every 1 minute (as currently configured).
## 2. Data Sourcing (API Integrations)
The workflow fetches data from two distinct news providers:
* GNews API: Searches for "AI" in English via the https://gnews.io endpoint.
* NewsAPI: Searches for "AI" sorted by publishedAt via the https://newsapi.org endpoint.
## 3. Data Processing & Merging
* Edit Fields (Set): Two nodes extract the $json.articles array from both API responses to standardize the input.
* Merge: Combines the article lists from both sources into a single data stream for the AI to analyze.
## 4. AI Intelligence (The "Brain")
* AI Agent: Orchestrates the summarization task.
* Model: Anthropic Chat Model using Claude Opus 4.7.
* The Prompt:
1. Select the top 15 most relevant articles on AI tech, tools, and research.
   2. Summarize each in 1-2 sentences.
   3. Include the URL for each.
   4. Prefix with today's date (yyyy/MM/dd).
   5. Constraint: Keep output under 2000 characters.
## 5. Delivery
* Node: Send a message (Gmail)
* Recipient: talktossaxena@gmail.com
* Content: The output generated by the AI Agent.

<img width="1030" height="340" alt="image" src="https://github.com/user-attachments/assets/38d9d294-de37-42b9-8508-f25a8d8ac3fb" />

# Social Media Content — n8n Workflow Documentation

## Overview

This workflow automatically generates platform-specific social media content (LinkedIn, Facebook, and Blog posts) whenever a new row is added to a Google Sheet. It uses Tavily to search the web for relevant articles, aggregates the results, and passes them through Claude-powered AI agents to produce ready-to-publish content — all written back to the same spreadsheet.

---

## Workflow Summary

| Property | Value |
|---|---|
| **Workflow Name** | Social Media Content |
| **Trigger** | Google Sheets — new row added |
| **AI Model** | Claude Opus 4.7 (Anthropic) |
| **Web Search** | Tavily API |
| **Output** | LinkedIn post, Facebook post, Blog article written back to Google Sheets |
| **Status** | Inactive (manual activation required) |

---

## Flow Diagram

```
Google Sheets Trigger
        ↓
Set Search Fields
        ↓
Search Web (Tavily API)
        ↓
Split Out (results)
        ↓
Aggregate (title + raw_content)
        ↓
LinkedIn Agent ──── Claude Opus 4.7
        ↓
Facebook Agent ──── Claude Opus 4.7
        ↓
Blog Post Agent ─── Claude Opus 4.7
        ↓
Update Row in Sheet
```

---

## Google Sheets Structure

The workflow reads from and writes to **Sheet1** of the `Social Media Content` Google Spreadsheet.

### Input Columns (filled before trigger fires)

| Column | Description |
|---|---|
| `Campaign` | Unique campaign identifier (used as the matching key for row updates) |
| `Content Subject` | The topic/query used to search the web (e.g. "AI trends in healthcare") |
| `Target Audience` | The professional audience the content is tailored for |

### Output Columns (written back by the workflow)

| Column | Description |
|---|---|
| `Linkdin` | Generated LinkedIn post |
| `Facebook` | Generated Facebook post |
| `Blog` | Generated two-paragraph blog article |

---

## Node-by-Node Breakdown

### 1. Google Sheets Trigger
- **Type:** `googleSheetsTrigger`
- **Event:** Row added (`rowAdded`)
- **Poll Interval:** Every minute
- **Purpose:** Monitors the spreadsheet and fires the workflow whenever a new row is detected.

---

### 2. Set Search Fields
- **Type:** `set`
- **Purpose:** Maps sheet columns to clean variable names for use downstream.
- **Mappings:**
  - `query` ← `Content Subject`
  - `targetAudience` ← `Target Audience`

---

### 3. Search Web
- **Type:** `httpRequest` (POST)
- **Endpoint:** `https://api.tavily.com/search`
- **Purpose:** Searches the web using the content subject as the query.
- **Tavily Parameters:**

| Parameter | Value |
|---|---|
| `search_depth` | `basic` |
| `topic` | `news` |
| `include_answer` | `true` |
| `include_raw_content` | `true` |
| `max_results` | `3` |

---

### 4. Split Out
- **Type:** `splitOut`
- **Field:** `results`
- **Purpose:** Splits Tavily's array of results into individual items for processing.

---

### 5. Aggregate
- **Type:** `aggregate`
- **Fields collected:** `title`, `raw_content`
- **Purpose:** Merges all split result items back into a single object containing all article titles and raw content, ready to pass to the AI agents.

---

### 6. LinkedIn Agent
- **Type:** `agent` (LangChain)
- **AI Model:** Claude Opus 4.7
- **Input:** Aggregated article content + target audience
- **Output field:** `output` → written to `Linkdin` column

**System Prompt Summary:**
> You are a professional LinkedIn content specialist. Create a LinkedIn post that is clear, engaging, mobile-optimized, tailored to the target audience, written in plain text with 1–2 emojis, includes a call to action, and ends with 3–5 relevant hashtags. Max 3,000 characters.

---

### 7. Facebook Agent
- **Type:** `agent` (LangChain)
- **AI Model:** Claude Opus 4.7
- **Input:** Aggregated article content + target audience
- **Output field:** `output` → written to `Facebook` column

**System Prompt Summary:**
> You are an expert Facebook content creator. Create a short, attention-grabbing post tailored to the audience with 1–2 emojis, a clear call to action, and 1–3 hashtags. Friendly and conversational tone.

---

### 8. Blog Post Agent
- **Type:** `agent` (LangChain)
- **AI Model:** Claude Opus 4.7
- **Input:** Aggregated article content + target audience
- **Output field:** `output` → written to `Blog` column

**System Prompt Summary:**
> You are a skilled blog writer. Write a two-paragraph blog article — engaging, coherent, and informative. Professional yet friendly tone. First paragraph introduces the topic; second delivers insights and conclusion.

---

### 9. Update Row in Sheet
- **Type:** `googleSheets` (update operation)
- **Matching Column:** `Campaign` (used to find the correct row)
- **Columns Updated:** `Linkdin`, `Facebook`, `Blog`, `Content Subject`, `Campaign`
- **Purpose:** Writes all three generated content pieces back into the original spreadsheet row.

---

## Credentials Required

| Credential | Used By |
|---|---|
| Google Sheets Trigger OAuth2 | Google Sheets Trigger |
| Google Sheets OAuth2 | Update row in sheet |
| Anthropic API | LinkedIn, Facebook, Blog Post agents |
| Tavily API Key | Search Web (hardcoded in HTTP body) |

> ⚠️ **Security Note:** The Tavily API key is currently hardcoded in the HTTP Request node body. It is recommended to move it to an n8n credential or environment variable before using in production.

---

## Setup Instructions

1. **Import** the `Social_Media_Content.json` workflow into your n8n instance.
2. **Connect credentials:**
   - Google Sheets (OAuth2) for both trigger and update nodes
   - Anthropic API key for all three AI agent nodes
   - Update the Tavily API key in the Search Web node (or store it as a credential)
3. **Prepare your Google Sheet** with the columns: `Campaign`, `Content Subject`, `Target Audience`, `Linkdin`, `Facebook`, `Blog`
4. **Activate** the workflow using the toggle in n8n.
5. **Add a new row** to the sheet with a campaign name, content subject, and target audience — the workflow will trigger automatically within a minute.

---

## Notes & Recommendations

- The workflow is currently **inactive** — you must manually activate it in n8n.
- The AI agents run **sequentially** (LinkedIn → Facebook → Blog), so each execution takes 3 API calls to Claude.
- To reduce Anthropic API costs, consider switching to **Claude Haiku** for faster and cheaper generation, especially for Facebook and Blog posts.
- The Tavily search is set to `"topic": "news"` — change this to `"general"` if your content subjects are not news-related.
- Poll interval is set to **every minute**, which may generate excessive trigger checks. Consider changing to `everyHour` or a scheduled time for lower overhead.

