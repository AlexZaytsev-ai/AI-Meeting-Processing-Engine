# AI Meeting Processing Engine

> Automatically processes voice meeting recordings using AI:
> transcribes audio, extracts action items, generates summaries, stores tasks, and creates Google Calendar events.


## 📌 Business Problem

After meetings, teams often spend time manually writing summaries, extracting action items, and updating task trackers and calendars. This process is repetitive, time-consuming, and prone to human error.

This workflow automates the entire meeting processing pipeline by converting voice recordings into text, extracting structured meeting information with AI, saving tasks to Google Sheets, creating Google Calendar events when meeting dates are detected, and sending a concise meeting summary back to Telegram.


## 🚀 Solution Overview

The workflow receives a voice message from Telegram, transcribes it into text using Whisper, analyzes the transcript with OpenAI, extracts structured meeting data, stores tasks in Google Sheets, creates Google Calendar events when meeting dates are detected, and sends a concise meeting summary back to Telegram.

The solution reduces manual work, improves consistency, and helps teams capture meeting outcomes automatically.


## 🏗 Workflow Architecture

![Workflow Architecture](workflow.png)


## 🔄 Workflow

```text
Telegram Voice Message
        │
        ▼
Speech-to-Text
        │
        ▼
AI Meeting Analysis
        │
        ▼
Prepare Data
        │
        ▼
Split Tasks
        ├── Google Calendar
        └── Google Sheets
        │
        ▼
Telegram Summary
```


## 🧠 Architecture Decisions

- Whisper is used for speech-to-text transcription.
- Structured Output guarantees predictable JSON responses.
- Each extracted task is processed independently.
- Google Calendar events are created only when meeting dates are detected.
- Google Sheets is used as persistent task storage.
- Telegram serves as the primary user interface.

## 💬 Demo

### 🎤 Input

Voice message sent via Telegram.

---

### 🤖 AI Output

**Summary**

> The project kickoff meeting was held with the client. Development will begin on Monday. The design team will prepare the initial mockups.

**Extracted Tasks**

- Alex — Prepare project structure — Monday
- Anna — Create UI mockups — Tuesday

---

### 📅 Google Calendar

Meeting event created automatically when a meeting date is detected.

---

### 📊 Google Sheets

All extracted tasks are automatically stored for further tracking.

---

### 📲 Telegram

A concise meeting summary is sent back to the user.
