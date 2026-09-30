# WhatsApp to Sheets

![High](/images/high.png)


> **⚠️ Hypothetical Scenario:**
>The business case and numbers presented in this README (the Lagos logistics company, hours saved, cost figures) are **illustrative examples** designed to show the *type* of problem this workflow solves and the *scale* of impact it could have. They are not based on a real company or actual measured data. Your real-world results will vary depending on message volume, business type, and setup. The workflow itself is real and fully functional.

---

## 🧪 Hypothetical Use Case

### The Scenario

Imagine a **small logistics company** in Lagos using WhatsApp as their primary order-taking channel. Customers message delivery details, addresses, and payment confirmations all day — across 3 business numbers.

At the end of each day, a staff member manually reads through hundreds of messages and types the data into a spreadsheet for dispatchers and billing.

| Task | Estimated Time |
|---|---|
| Reading & copying messages to sheet | ~2.5 hrs/day |
| Fixing copy-paste errors & missed orders | ~45 min/day |
| Staff cost (at ₦2,000/hr equivalent) | ~$4/day |

**Hypothetically: ~$80/month and 65+ hours lost to pure data entry**

### With N8N Workflow

1. Staff exports the WhatsApp chat (takes 10 seconds on any phone)
2. Drops the `.txt` file into a watched Google Drive folder
3. n8n parses every message — extracting date, time, sender, message body
4. Rows appended to Google Sheets instantly
5. An IF node flags messages with keywords like `"order"`, `"deliver"`, `"address"` for quick review

### Projected Impact (Illustrative)

| Metric | Before (manual) | After (automated) |
|---|---|---|
| Daily entry time | ~3.25 hrs | ~10 min (review only) |
| Monthly hours recovered | — | **~65 hrs** |
| Monthly cost saved | — | **~$80** |
| Data entry errors | ~12/week | ~0/week |
| Staff satisfaction | 😩 | 😄 |

---

## 🏭 Where This Automation Actually Helps

This is not a one-trick workflow. Any person or team that **receives structured information over WhatsApp and then manually copies it somewhere** is a perfect candidate.

> 💡 **The common pattern:** Someone sends useful info over WhatsApp → someone else manually copies it into a spreadsheet → errors, delays, and wasted hours pile up. This workflow eliminates that middle step.

| # | Industry / Role | The Problem | How This Helps | Keywords to Flag |
|---|---|---|---|---|
| 📦 | **Small Business Ops** | Owners in emerging markets run their entire order pipeline over WhatsApp. No CRM, no forms — just messages. | Every order gets parsed, timestamped, and logged automatically. Sheet is ready before the owner's morning coffee. | `order`, `deliver`, `quantity`, `address` |
| 🏥 | **NGOs & Health Workers** | Field workers report daily case counts, supply requests, or patient data via group chats. A coordinator manually collates everything. | Export the group chat → every field report lands in a database in minutes, ready for donor dashboards or analysis. | `cases`, `supply`, `urgent`, `report` |
| 🏫 | **Schools & Training Programs** | Attendance, fee confirmations, and homework submissions come through parent WhatsApp groups. Admin types it all in manually. | Parse parent messages for keywords and auto-populate attendance or payment trackers. | `paid`, `present`, `absent`, `confirmed` |
| 🛒 | **Market Vendors & Cooperatives** | Bulk order coordination, stock availability, and payment confirmations all live in informal group threads. | Pull orders and stock requests into a shared sheet — multiple vendors, zero manual entry. | `stock`, `bulk`, `available`, `payment` |
| 🏗️ | **Freelancers & Agencies** | Client briefs, revision requests, and approvals arrive over WhatsApp. Tracking what was agreed, when, and by whom is chaotic. | Archive conversation logs into a structured Notion or Sheets timeline — useful for billing disputes and project audits. | `approved`, `revise`, `deadline`, `brief` |
| 🚗 | **Fleet & Delivery Dispatchers** | Small operators (outside Uber/Bolt) coordinate drivers via WhatsApp. Trip requests and delivery statuses happen entirely in chat. | Log trip confirmations with timestamps, flag completed deliveries, maintain a daily dispatch log automatically. | `pickup`, `delivered`, `en route`, `done` |
| 📊 | **Researchers & Journalists** | Qualitative researchers collect interview responses over WhatsApp. Organising quotes, timestamps, and sources manually takes hours. | Export interview chats → parse into a structured dataset of speaker, timestamp, quote — ready for analysis or fact-checking. | `quote`, `source`, `confirmed`, `off record` |
---

## ⚙️ How It Works

![HighLevel](/images/highlevel.png)

## Workflow Breakdown
```
WhatsApp .txt Export
        │
        ▼
  n8n File Trigger          ← watches a folder (Google Drive or local)
        │
        ▼
  Read File Node            ← loads raw text content
        │
        ▼
  Code Node (JS)            ← regex parses each line → {date, time, sender, message}
        │
        ▼
  IF Filter Node            ← flags keywords: "order", "deliver", "address"
        │
        ▼
  Google Sheets Node        ← appends structured rows
        │
        ▼
  Notify (optional)         ← Slack / Email / Telegram summary
```

---

## 🗂️ WhatsApp Export Format

WhatsApp exports chats as `.txt` files like this:

```
[24/03/2024, 09:14:22] John Doe: I need 5 bags delivered to Lekki Phase 1
[24/03/2024, 09:16:05] Jane Smith: Please confirm order #442
[24/03/2024, 09:20:11] John Doe: Payment sent via transfer
```

The Code node maps every line to structured columns:

| Column | Example |
|---|---|
| `date` | 24/03/2024 |
| `time` | 09:14:22 |
| `sender` | John Doe |
| `message` | I need 5 bags delivered to Lekki Phase 1 |
| `flagged` | TRUE |

---

## 🔧 Customization Ideas

| Goal | How |
|---|---|
| Store in Airtable | Swap Google Sheets node for Airtable node |
| Filter only orders | Add IF node checking for order keywords |
| Handle multiple exports at once | Loop over files with n8n's Split In Batches node |
| Send a daily digest | Add Schedule Trigger + summarize rows with a Code node |
| Support other locales | Update date regex in the Code node for your region |
| Push to Notion | Use the Notion node to create a database entry per message |
| Push to Supabase / MySQL | Use the HTTP Request or Postgres node |

---
## 🎬 Demo Video

<div>
    <a href="https://www.loom.com/share/b99f0430ba5a480489c60af41434b765">
    </a>
    <a href="https://www.loom.com/share/b99f0430ba5a480489c60af41434b765">
      <img style="max-width:300px;" src="https://cdn.loom.com/sessions/thumbnails/b99f0430ba5a480489c60af41434b765-4813f095d8d6c4a8-full-play.gif#t=0.1">
    </a>
  </div>

  *Note: Video generated using Notebook LM. Potential errors may exist.*