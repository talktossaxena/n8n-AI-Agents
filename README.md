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

<img width="1015" height="185" alt="image" src="https://github.com/user-attachments/assets/88121410-53de-40c3-bf7b-d9d59dc45cde" />

# Email and Meeting — n8n Workflow Documentation

## Overview

This workflow is a conversational **AI Agent** that listens for chat messages and autonomously performs actions across Gmail, Google Calendar, and Google Sheets — all from natural language instructions. The agent uses **Claude Haiku 4.5** to reason about the user's request and decides which tool(s) to invoke without any manual routing.

---

## Workflow Diagram

> *(Add your workflow screenshot here)*

---

## Workflow at a Glance

| Property | Value |
|---|---|
| **Name** | Email and Meeting |
| **Trigger** | Chat message (n8n Chat UI / webhook) |
| **AI Model** | Claude Haiku 4.5 (`claude-haiku-4-5-20251001`) |
| **Tools Available** | 7 tools across Gmail, Google Calendar, Google Sheets |
| **Status** | Inactive (requires manual activation) |
| **Execution Order** | v1 |

---

## Node Execution Flow

```
┌──────────────────────────────┐
│  When chat message received  │  ← User sends a message in n8n Chat UI
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────────────────────────┐
│                   AI Agent                       │
│           (Claude Haiku 4.5)                     │
│                                                  │
│  Tools:                                          │
│  ├─ 1. Send a message in Gmail                   │
│  ├─ 2. Create an event in Google Calendar        │
│  ├─ 3. Get many labels in Gmail                  │
│  ├─ 4. Add label to message in Gmail             │
│  ├─ 5. Get many messages in Gmail                │
│  └─ 6. Append row in sheet in Google Sheets      │
└──────────────────────────────────────────────────┘
               ↓
    Agent picks the right tool(s) based on
    the user's natural language message
```

---

## Nodes Overview

| # | Node | Type | Purpose |
|---|---|---|---|
| 1 | When chat message received | Trigger | Entry point — receives user message |
| 2 | AI Agent | Agent | Reasons and decides which tools to call |
| 3 | Anthropic Chat Model | LLM | Powers the agent with Claude Haiku 4.5 |
| 4 | Send a message in Gmail | Tool | Composes and sends an email |
| 5 | Create an event in Google Calendar | Tool | Creates a calendar event |
| 6 | Get many labels in Gmail | Tool | Fetches all available Gmail labels |
| 7 | Add label to message in Gmail | Tool | Applies a label to a specific email |
| 8 | Get many messages in Gmail | Tool | Retrieves the latest email(s) from inbox |
| 9 | Append row in sheet in Google Sheets | Tool | Logs email details to a spreadsheet |
| 10 | Sticky Note | Note | Visual annotation on the canvas |

---

## Node Details

### 1. When Chat Message Received
| Property | Value |
|---|---|
| **Node Type** | `chatTrigger` (LangChain, v1.4) |
| **Webhook ID** | `089e2a22-e368-4b80-8060-e55766bf67d5` |

Listens for incoming chat messages and passes them to the AI Agent as user input.

**Example messages this workflow handles:**
- *"Send an email to alex@company.com about tomorrow's meeting"*
- *"Schedule a call from 3pm to 4pm today"*
- *"Label my last email as Important"*
- *"Get my latest email and log it to the sheet"*

---

### 2. AI Agent
| Property | Value |
|---|---|
| **Node Type** | `agent` (LangChain, v3.1) |
| **Model** | Anthropic Chat Model (Claude Haiku 4.5) |
| **Tools Connected** | 6 tools (see below) |

The brain of the workflow. Receives the user's chat message, determines what action(s) to take, calls the right tool(s) in the correct sequence, and returns a response.

**Multi-step example:**
> *"Label my last email as Follow Up"*
> 1. Calls **Get many messages** → gets latest email ID
> 2. Calls **Get many labels** → finds the "Follow Up" label ID
> 3. Calls **Add label to message** → applies the label

---

### 3. Anthropic Chat Model
| Property | Value |
|---|---|
| **Node Type** | `lmChatAnthropic` (v1.5) |
| **Model** | `claude-haiku-4-5-20251001` |
| **Credential** | Anthropic account |

Provides language understanding and reasoning for the AI Agent. Claude Haiku 4.5 is chosen for speed and cost-efficiency.

---

### 4. Send a Message in Gmail
| Property | Value |
|---|---|
| **Node Type** | `gmailTool` (v2.2) |
| **Operation** | Send message |
| **Credential** | Gmail OAuth2 |

Composes and sends an email. All fields are filled by the AI from the user's message.

| Parameter | Filled By |
|---|---|
| `To` | AI extracts recipient from user message |
| `Subject` | AI generates based on context |
| `Message` | AI composes the full email body |

---

### 5. Create an Event in Google Calendar
| Property | Value |
|---|---|
| **Node Type** | `googleCalendarTool` (v1.3) |
| **Operation** | Create event |
| **Calendar** | `talktossaxena@gmail.com` |
| **Credential** | Google Calendar OAuth2 |

Creates a calendar event. The AI parses natural language time expressions into correct datetime formats.

| Parameter | Filled By |
|---|---|
| `Start` | AI parses from message (e.g. "tomorrow 2pm") |
| `End` | AI parses from message (e.g. "3pm") |

---

### 6. Get Many Labels in Gmail
| Property | Value |
|---|---|
| **Node Type** | `gmailTool` (v2.2) |
| **Operation** | `getAll` (resource: label) |
| **Credential** | Gmail OAuth2 |

Fetches all Gmail labels from the user's account. The agent calls this before applying a label to ensure it uses the correct label ID.

---

### 7. Add Label to Message in Gmail
| Property | Value |
|---|---|
| **Node Type** | `gmailTool` (v2.2) |
| **Operation** | `addLabels` |
| **Credential** | Gmail OAuth2 |

Applies one or more labels to a specific Gmail message.

| Parameter | Filled By |
|---|---|
| `Message_ID` | AI retrieves from Get Many Messages tool |
| `Label_Names_or_IDs` | AI retrieves from Get Many Labels tool |

---

### 8. Get Many Messages in Gmail
| Property | Value |
|---|---|
| **Node Type** | `gmailTool` (v2.2) |
| **Operation** | `getAll` |
| **Limit** | `1` (most recent email) |
| **Credential** | Gmail OAuth2 |

Fetches the latest email from the inbox. This is the key tool that enables the agent to act on the *last received email* without asking the user for a Message ID.

**Typical agent flow using this tool:**
```
User: "Add 'Follow Up' label to my last email"
  → Agent calls Get Many Messages (limit 1)
  → Extracts message ID from result
  → Agent calls Get Many Labels
  → Extracts label ID for 'Follow Up'
  → Agent calls Add Label to Message
  → ✅ Done
```

---

### 9. Append Row in Sheet in Google Sheets
| Property | Value |
|---|---|
| **Node Type** | `googleSheetsTool` (v4.7) |
| **Operation** | Append row |
| **Spreadsheet** | `email details` |
| **Sheet** | `Sheet1` |
| **Credential** | Google Sheets OAuth2 |

Logs email information to a Google Sheet. Useful for keeping a record of sent or received emails.

| Column | Filled By |
|---|---|
| `Email Title` | AI extracts/generates the email subject |
| `Email Message` | AI extracts/generates the email body |

---

## Credentials Required

| Credential | Node(s) |
|---|---|
| **Anthropic API** | Anthropic Chat Model |
| **Gmail OAuth2** | Send message, Get labels, Add label, Get messages |
| **Google Calendar OAuth2** | Create an event in Google Calendar |
| **Google Sheets OAuth2** | Append row in Google Sheets |

---

## Setup Guide

### Step 1 — Import Workflow
- In n8n: **Workflows → Import from file → `email_workflow.json`**

### Step 2 — Connect Credentials

**Anthropic**
- **Credentials → New → Anthropic API**
- Paste your key from [console.anthropic.com](https://console.anthropic.com)

**Gmail (OAuth2)**
- **Credentials → New → Gmail OAuth2**
- Sign in with your Google account
- Required scope: `https://mail.google.com/`

**Google Calendar (OAuth2)**
- **Credentials → New → Google Calendar OAuth2**
- Sign in with the calendar owner's Google account
- Enable **Google Calendar API** in [Google Cloud Console](https://console.cloud.google.com)
- Update the calendar field from `talktossaxena@gmail.com` to your own calendar

**Google Sheets (OAuth2)**
- **Credentials → New → Google Sheets OAuth2**
- Sign in with the Google account that owns the `email details` spreadsheet
- Ensure `Sheet1` has columns: `Email Title`, `Email Message`

### Step 3 — Activate
- Toggle the workflow **Active** in the top-right corner

### Step 4 — Test via Chat
Open the **Chat** panel and try:
```
Label my last email as Important
```
```
Send an email to test@example.com saying the meeting is at 3pm
```
```
Schedule a meeting tomorrow from 10am to 11am
```
```
Log my last email to the sheet
```

---

## Example Use Cases

| User Message | Tools Called |
|---|---|
| *"Email John at john@co.com about the report"* | Send message in Gmail |
| *"Block my calendar 2pm–3pm today"* | Create event in Google Calendar |
| *"Label my last email as Follow Up"* | Get messages → Get labels → Add label |
| *"What labels do I have in Gmail?"* | Get many labels |
| *"Log my last email to the sheet"* | Get messages → Append row in Sheets |
| *"Email Sarah and create a meeting for tomorrow"* | Send message + Create event |

<img width="675" height="250" alt="image" src="https://github.com/user-attachments/assets/4f0a43ac-37d3-40a6-a6f3-ba60de6369be" />


# Telegram Integration with AI Agent — n8n Workflow

## Overview

This workflow connects a **Telegram Bot** to an **AI Agent powered by OpenAI**, allowing users to send messages to the bot and receive intelligent AI-generated replies automatically.

```
Telegram Trigger → AI Agent → Send a text message
                      ↑
               OpenAI Chat Model
```

---

## Workflow Nodes

### 1. Telegram Trigger
| Property | Value |
|---|---|
| Type | `telegramTrigger` |
| Trigger On | `message` |
| Credential | Telegram account |

Listens for incoming messages sent to your Telegram bot. Every time a user sends a message, this node fires and passes the message data to the next node.

**Output data includes:**
- `message.text` — the user's message text
- `message.chat.id` — the chat ID used to send the reply back

---

### 2. AI Agent
| Property | Value |
|---|---|
| Type | `@n8n/n8n-nodes-langchain.agent` |
| Prompt | `{{ $json.message.text }}` |
| Language Model | OpenAI Chat Model (connected below) |

Takes the user's message text from the Telegram Trigger and sends it to the AI Agent. The agent processes the input using the connected OpenAI model and generates a response.

---

### 3. OpenAI Chat Model
| Property | Value |
|---|---|
| Type | `lmChatOpenAi` |
| Model | `gpt-5-mini` |
| Credential | OpenAI account |

The language model powering the AI Agent. Connected to the AI Agent node as a sub-node (`ai_languageModel`). Processes the prompt and returns a text response.

---

### 4. Send a Text Message
| Property | Value |
|---|---|
| Type | `telegram` |
| Chat ID | `{{ $('Telegram Trigger').item.json.message.chat.id }}` |
| Text | `{{ $json.output }}` |
| Credential | Telegram account |

Sends the AI Agent's response back to the user on Telegram. Uses the `chat.id` from the original message to reply to the correct conversation.

---

## Data Flow

```
User sends message on Telegram
        ↓
Telegram Trigger captures message.text
        ↓
AI Agent receives text as prompt
        ↓
OpenAI Chat Model (gpt-5-mini) processes it
        ↓
AI Agent outputs response in $json.output
        ↓
Send a Text Message → replies to user on Telegram
```

---

## Credentials Required

| Credential | Used By | Purpose |
|---|---|---|
| **Telegram account** | Telegram Trigger + Send a text message | Bot token to receive & send messages |
| **OpenAI account** | OpenAI Chat Model | API key to call GPT model |

---

## Setup Requirements

Before running this workflow you need:

1. **A Telegram Bot Token** — create a bot via [@BotFather](https://t.me/botfather) on Telegram
2. **An OpenAI API Key** — get one from [platform.openai.com](https://platform.openai.com)
3. **A public HTTPS webhook URL** — required by Telegram to deliver messages to n8n (e.g. via Cloudflare Tunnel or ngrok)
4. Set `WEBHOOK_URL` environment variable in n8n to your public HTTPS URL before starting

---

## Workflow Settings

| Setting | Value |
|---|---|
| Status | Active |
| Execution Order | v1 |
| Binary Mode | separate |

---

## Notes

- The workflow is set to **active**, meaning it runs automatically in production when a Telegram message is received.
- Only **one Telegram Trigger per bot** is allowed at a time (Telegram API limitation).
- The AI Agent uses `gpt-5-mini` — you can swap this for `gpt-4o` or other models in the OpenAI Chat Model node for different performance/cost trade-offs.
- To add memory, tools, or custom instructions to the AI Agent, connect additional sub-nodes (e.g. Memory, Tool nodes) to the AI Agent node.

