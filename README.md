# sheet-tracker-n8n
i created a n8n agent to track click count of urls


# 🎥 YouTube URL QR & Click Tracking Workflows  

This project contains two connected **n8n workflows** for automating the generation of **redirect URLs + QR codes** for YouTube videos, and for tracking **QR scans and click counts** in Google Sheets.  

- **`urlcreator` workflow** → Reads YouTube links from a Google Sheet, normalizes them, and generates **redirect URLs & QR codes**.  
- **`finalcloud` workflow** → Handles incoming QR scans via a webhook, redirects to the correct YouTube video, and logs **click count & metadata** (IP, user-agent, timestamp) back to Google Sheets.  

---

## 📑 Table of Contents
- [Introduction](#introduction)  
- [Workflows](#workflows)  
  - [1. URL Creator (`urlcreator`)](#1-url-creator-urlcreator)  
  - [2. QR Scan & Tracking (`finalcloud`)](#2-qr-scan--tracking-finalcloud)  
- [Installation](#installation)  
- [Usage](#usage)  
- [License](#license)  

---

## 📌 Introduction
These workflows enable you to:  
- Automate **YouTube link cleanup & normalization** (handling `youtu.be`, `shorts`, `watch?v=`, and playlists).  
- Generate **redirect URLs** that funnel clicks through a tracking webhook.  
- Create **QR codes** for each video link.  
- Track **clicks, user agent, IP, timestamp, and total scan counts** in Google Sheets.  

Perfect for campaigns, classrooms, or organizations where you need to distribute **scannable video links** while collecting analytics.  

---

## ⚡ Workflows  

### 1. URL Creator (`urlcreator`)
- **Trigger**: Manual Trigger  
- **Steps**:
  1. **Read All Videos** → Fetch rows from Google Sheets (`Videos!A1:H999`).  
  2. **Generate URLs** →  
     - Normalize YouTube URLs.  
     - Generate **redirect URL** pointing to the webhook.  
     - Generate a **QR code** using `api.qrserver.com`.  
  3. **Update Videos Sheet** → Write `Redirect URL` and `QR Code URL` back to Google Sheets.  

✅ Skips headers & empty rows.  
✅ Handles multiple YouTube URL formats.  

---

### 2. QR Scan & Tracking (`finalcloud`)
- **Trigger**: Webhook (`/track-scan`).  
- **Steps**:  
  1. **Process Click Data** → Extract `id`, `target`, user-agent, IP.  
  2. **Redirect to YouTube** → Sends user to the final YouTube link.  
  3. **Log to Google Sheets** → Append click event details.  
  4. **Get Existing Row** → Find the video by `ID`.  
  5. **Increment Count** → Increase `Click Count` for that video.  
  6. **Update Click Count** → Save back into the sheet.  

✅ Stores timestamp, date, time, user agent, and IP.  
✅ Increments click count on each scan.  

---

## 🛠 Installation
1. Install [n8n](https://n8n.io).  
2. Import both JSON workflows into n8n:  
   - `urlcreator_final.json`  
   - `finalcloud_final.json`  
3. Set up Google Sheets credentials in n8n.  
4. Configure your Google Sheet with the correct structure (see below).  
5. Deploy the workflows (ensure `finalcloud` webhook is active).  

---

## 🚀 Usage
1. **Populate Google Sheets** with video IDs & YouTube URLs.  
2. **Run `urlcreator` workflow** → Fills sheet with redirect & QR code links.  
3. Share the **QR codes** publicly.  
4. When users scan, `finalcloud` workflow:  
   - Redirects them to the YouTube video.  
   - Logs the scan details.  
   - Increments the click counter.  

---

## 📜 License
This project is open source under the **MIT License**.  
