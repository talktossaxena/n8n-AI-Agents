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

This workflow automatically generates platform-specific social media content — **LinkedIn posts**, **Facebook posts**, and **Blog articles** — whenever a new row is added to a Google Sheet. It uses the **Tavily API** to search the web for relevant articles, aggregates the results, and passes them through **Claude Haiku 4.5**-powered AI agents to produce ready-to-publish content, which is then written back into the same spreadsheet row.

---

## Workflow at a Glance

| Property | Value |
|---|---|
| **Name** | Social Media Content |
| **Trigger** | Google Sheets — new row added (polls every minute) |
| **AI Model** | Claude Haiku 4.5 (`claude-haiku-4-5-20251001`) |
| **Web Search** | Tavily API (`https://api.tavily.com/search`) |
| **Output Destinations** | LinkedIn, Facebook, Blog columns in Google Sheets |
| **Status** | Inactive (requires manual activation) |
| **Execution Order** | v1 (sequential) |

---

## Node Execution Flow

```
┌─────────────────────────┐
│   Google Sheets Trigger │  ← Fires on every new row added
└────────────┬────────────┘
             ↓
┌────────────────────────┐
│    Set Search Fields   │  ← Maps "Content Subject" → query
└────────────┬───────────┘     Maps "Target Audience" → targetAudience
             ↓
┌────────────────────────┐
│     Search Web         │  ← POST to Tavily API (max 3 results)
└────────────┬───────────┘
             ↓
┌────────────────────────┐
│       Split Out        │  ← Splits results array into individual items
└────────────┬───────────┘
             ↓
┌────────────────────────┐
│       Aggregate        │  ← Collects title + raw_content from all items
└────────────┬───────────┘
             ↓
┌────────────────────────┐
│    LinkedIn Agent      │  ← Claude Haiku 4.5
└────────────┬───────────┘
             ↓
┌────────────────────────┐
│    Facebook Agent      │  ← Claude Haiku 4.5
└────────────┬───────────┘
             ↓
┌────────────────────────┐
│    Blog Post Agent     │  ← Claude Haiku 4.5
└────────────┬───────────┘
             ↓
┌────────────────────────┐
│  Update Row in Sheet   │  ← Writes LinkedIn, Facebook, Blog back to sheet
└────────────────────────┘
```

---

## Google Sheets Structure

**Spreadsheet:** `Social Media Content`  
**Sheet:** `Sheet1` (`gid=0`)

### Input Columns *(filled by user before trigger fires)*

| Column | Description |
|---|---|
| `Campaign` | Unique campaign name — used as the **row matching key** for updates |
| `Content Subject` | The topic to search (e.g. `"AI in healthcare 2025"`) |
| `Target Audience` | The intended audience (e.g. `"HR professionals"`) |

### Output Columns *(written back by the workflow)*

| Column | Source Node | Description |
|---|---|---|
| `Linkdin` | LinkedIn Agent | Generated LinkedIn post (plain text, ≤3,000 chars) |
| `Facebook` | Facebook Agent | Generated Facebook post (short, mobile-friendly) |
| `Blog` | Blog Post Agent | Two-paragraph blog article |

---

## Node Details

### 1. Google Sheets Trigger
| Property | Value |
|---|---|
| **Node Type** | `googleSheetsTrigger` |
| **Event** | `rowAdded` |
| **Poll Interval** | Every minute |
| **Credential** | Google Sheets Trigger OAuth2 |

Watches the spreadsheet and triggers the workflow when a new row is detected.

---

### 2. Set Search Fields
| Property | Value |
|---|---|
| **Node Type** | `set` (v3.4) |

Maps raw spreadsheet column names to clean variables:

| Output Variable | Source Column |
|---|---|
| `query` | `Content Subject` |
| `targetAudience` | `Target Audience` |

---

### 3. Search Web
| Property | Value |
|---|---|
| **Node Type** | `httpRequest` (v4.4) |
| **Method** | POST |
| **Endpoint** | `https://api.tavily.com/search` |

**Tavily Request Parameters:**

| Parameter | Value |
|---|---|
| `query` | `{{ $json.query }}` (from Set Search Fields) |
| `search_depth` | `basic` |
| `topic` | `news` |
| `include_answer` | `true` |
| `include_raw_content` | `true` |
| `max_results` | `3` |

> ⚠️ **Security:** The Tavily API key is currently hardcoded in the request body. Move it to an n8n credential or environment variable before deploying to production.

---

### 4. Split Out
| Property | Value |
|---|---|
| **Node Type** | `splitOut` |
| **Field** | `results` |

Splits Tavily's `results` array into individual items so each article can be processed separately before aggregation.

---

### 5. Aggregate
| Property | Value |
|---|---|
| **Node Type** | `aggregate` |
| **Mode** | Aggregate all item data |
| **Fields Collected** | `title`, `raw_content` |

Merges all split result items into a single JSON object passed to the AI agents.

---

### 6. LinkedIn Agent
| Property | Value |
|---|---|
| **Node Type** | `agent` (LangChain, v3.1) |
| **Model** | Claude Haiku 4.5 |
| **Output** | `$('Linkdein').item.json.output` |

**System Prompt Instructions:**
- Role: Professional LinkedIn content specialist
- Output: Plain text only, no markdown formatting
- Length: ≤ 3,000 characters
- Must include: 1–2 emojis, a call to action, 3–5 hashtags
- Tone: Human, conversational, mobile-optimised
- Tailored to the `targetAudience` field

---

### 7. Facebook Agent
| Property | Value |
|---|---|
| **Node Type** | `agent` (LangChain, v3.1) |
| **Model** | Claude Haiku 4.5 |
| **Output** | `$('Facebook').item.json.output` |

**System Prompt Instructions:**
- Role: Expert Facebook content creator
- Output: Short, attention-grabbing post only
- Must include: 1–2 emojis, a clear call to action, 1–3 hashtags
- Tone: Friendly, conversational, mobile-first
- Spark interaction (likes, comments, shares)

---

### 8. Blog Post Agent
| Property | Value |
|---|---|
| **Node Type** | `agent` (LangChain, v3.1) |
| **Model** | Claude Haiku 4.5 |
| **Output** | `$json.output` |

**System Prompt Instructions:**
- Role: Skilled blog writer
- Output: Exactly two paragraphs
- Paragraph 1: Introduction — hook the reader, introduce the topic
- Paragraph 2: Insight + conclusion — deliver value, end with a takeaway
- Tone: Professional yet friendly
- Coherent, engaging, and informative for a general audience

---

### 9. Update Row in Sheet
| Property | Value |
|---|---|
| **Node Type** | `googleSheets` (v4.7) |
| **Operation** | Update |
| **Matching Column** | `Campaign` |
| **Credential** | Google Sheets OAuth2 |

**Column Mapping:**

| Sheet Column | Value Source |
|---|---|
| `Campaign` | `$('Google Sheets Trigger').last().json.Campaign` |
| `Content Subject` | `$('Google Sheets Trigger').last().json['Content Subject']` |
| `Linkdin` | `$('Linkdein').item.json.output` |
| `Facebook` | `$('Facebook').item.json.output` |
| `Blog` | `$json.output` |

---

## Credentials Required

| Credential | Node(s) |
|---|---|
| **Google Sheets Trigger OAuth2** | Google Sheets Trigger |
| **Google Sheets OAuth2** | Update row in sheet |
| **Anthropic API** | LinkedIn Agent, Facebook Agent, Blog Post Agent |
| **Tavily API Key** | Search Web (currently hardcoded — move to credential) |

---

## Setup Guide

1. **Import** `Social_Media_Content.json` into your n8n instance via *Workflows → Import from file*
2. **Set up credentials:**
   - Connect a Google account with Sheets access for both the trigger and update nodes
   - Add your Anthropic API key under *Credentials → Anthropic*
   - Replace the hardcoded Tavily API key in the Search Web node with your own key
3. **Prepare your Google Sheet** with these exact column headers:
   ```
   Campaign | Content Subject | Target Audience | Linkdin | Facebook | Blog
   ```
4. **Activate** the workflow using the toggle in the top-right of the editor
5. **Add a row** to the sheet — the workflow will trigger within one minute and populate the content columns automatically
