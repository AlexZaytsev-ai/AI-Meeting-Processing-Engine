# AI Meeting Processing Engine

[Русская версия](README_RU.md)

An n8n workflow that transforms Telegram voice recordings into structured meeting summaries, tasks, and follow-up calendar events.

![Workflow architecture](workflow-overview.png)

## Business Problem

Meeting results are often processed manually: someone has to transcribe the recording, prepare a summary, extract tasks, and schedule the next meeting. This takes time and increases the risk of losing important information.

## Solution

The workflow:

* receives a voice message through Telegram;
* rejects unsupported message types;
* prevents duplicate processing;
* transcribes the audio;
* analyzes the transcript with GPT-5.1;
* returns structured meeting data;
* sends a summary to Telegram;
* saves each task as a separate Google Sheets row;
* creates a Google Calendar event only when both date and time are available.

## Workflow Architecture

```text
Voice Trigger
    ↓
Check Voice Message
    ↓
Prevent Duplicate Processing
    ↓
Download Voice
    ↓
Speech to Text
    ↓
AI Meeting Analysis
    ↓
Prepare Data
    ├── Send Summary → Telegram
    ├── Split Tasks → Google Sheets
    └── Check Date & Time → Google Calendar
```

The Telegram, task storage, and calendar branches run independently after the meeting data is prepared.

## Key Architecture Decisions

* A regular OpenAI model node is used instead of an AI Agent because the model analyzes data but does not select tools.
* Strict JSON Schema ensures predictable structured output.
* Tasks are processed independently from calendar creation.
* Calendar events require both a date and a time.
* Duplicate voice messages are discarded using Telegram `chat.id` and `file_unique_id`.
* Processing errors and integration errors use separate notification branches.
* Credentials, document IDs, calendar IDs, and webhook identifiers are excluded from the public workflow file.

## Tested Scenarios

* A regular text message is rejected with a request to send a voice recording.
* A meeting with tasks and a follow-up date creates Telegram, Google Sheets, and Calendar outputs.
* Repeated processing of the same voice message is prevented.
* Tasks are saved even when no follow-up meeting is scheduled.
* A date without a time does not create an invalid calendar event.

## Tech Stack

* n8n
* Telegram Bot API
* OpenAI Speech-to-Text
* OpenAI GPT-5.1
* JSON Schema / Structured Outputs
* Google Sheets API
* Google Calendar API

## Setup

1. Import `AI-Meeting-Processing-Engine.public.json` into n8n.
2. Configure Telegram, OpenAI, Google Sheets, and Google Calendar credentials.
3. Replace the Google Sheet and Calendar placeholders with your own resources.
4. Activate the workflow and send a Telegram voice message.

## Repository Structure

```text
.
├── AI-Meeting-Processing-Engine.public.json
├── workflow-overview.png
├── README.md
├── README_RU.md
└── LICENSE
```

## Author

Alexander Zaytsev
Junior AI Automation Engineer
[GitHub Profile](https://github.com/AlexZaytsev-ai)
