# WhatsApp AI Voice & Text Assistant using n8n (AWS Hosted)


<img width="1919" height="951" alt="Screenshot 2026-07-13 144727" src="https://github.com/user-attachments/assets/aa02a43f-7e8c-4502-8f13-732751a8ccb6" />



## Overview

This project is an AI-powered WhatsApp Assistant hosted on **AWS** using **n8n**, **OpenAI**, **OpenRouter**, and the **WhatsApp Cloud API**.

The system can:

* Receive WhatsApp messages in real-time
* Detect whether the message is:

  * Text
  * Voice
* Convert voice to text using AI
* Process conversations using an AI Agent
* Generate intelligent replies
* Send responses back as:

  * Text
  * AI-generated voice

The entire workflow is automated using n8n and deployed on AWS infrastructure for scalability and reliability.

---

# System Architecture

## Cloud Infrastructure

This project is hosted on **Amazon Web Services (AWS)**.

### AWS Responsibilities

AWS is used for:

* Hosting n8n server
* Running workflow automation
* Managing API requests
* Handling webhook traffic
* Providing scalable infrastructure
* Ensuring uptime and reliability

Typical deployment options:

* EC2
* Docker on AWS
* Reverse proxy with Nginx
* SSL-secured webhook endpoints

---

# Core Technologies

## Automation Platform

* n8n

## Cloud Hosting

* AWS

## AI Services

* OpenAI
* OpenRouter

## Messaging Platform

* WhatsApp Cloud API

## AI Features

* Speech-to-Text
* Text Generation
* Text-to-Speech

## External APIs

* OpenWeatherMap API

---

# Workflow Overview

```txt
WhatsApp User
      ↓
WhatsApp Cloud API
      ↓
AWS Hosted n8n Workflow
      ↓
Detect Message Type
      ↓
Voice → Transcribe Audio
Text → Direct Processing
      ↓
AI Agent Processing
      ↓
Generate Response
      ↓
Text Reply / Voice Reply
      ↓
WhatsApp User
```

---

# Detailed Workflow

# 1. WhatsApp Trigger

### Node:

`WhatsApp Trigger`

The workflow begins when a user sends:

* A text message
* A voice note

The webhook endpoint hosted on AWS receives incoming events from WhatsApp Cloud API.

---

# 2. Voice Detection

### Node:

`Is Voice?`

Checks whether the incoming message contains audio data.

Logic:

```js
!! $json.messages[0].audio
```

If true:

* Workflow enters voice processing flow

Else:

* Workflow enters text processing flow

---

# 3. Message Routing

### Node:

`Switch`

Routes workflow into:

* Voice workflow
* Text workflow

---

# Voice Processing Pipeline

# 4. Download WhatsApp Media

### Node:

`Download media`

Fetches media URL using:

* WhatsApp Media API

Input:

```json
messages[0].audio.id
```

---

# 5. Download Audio Binary

### Node:

`Download Audio File`

Downloads actual voice file from WhatsApp servers.

Authentication:

* WhatsApp API Credentials

---

# 6. Audio Transcription

### Node:

`Transcribe a recording`

Uses OpenAI Speech-to-Text model.

Purpose:

* Converts voice message into plain text

This enables the AI system to understand spoken conversations.

---

# Text Processing Pipeline

# 7. Prepare Unified Input

### Node:

`Agent Input`

Combines:

* Voice transcript
* Direct text input

This creates a unified processing layer for the AI Agent.

Expression:

```js
{{ $json.text }}{{ $('WhatsApp Trigger').item.json.messages[0].text.body }}
```

---

# AI Intelligence Layer

# 8. AI Agent

### Node:

`AI Agent`

This is the central conversational AI engine.

Responsibilities:

* Understand user intent
* Generate smart responses
* Maintain conversation flow
* Use connected tools dynamically

---

# 9. AI Model Integration

### Node:

`OpenRouter Chat Model`

Model:

```txt
openai/gpt-4o-mini
```

Purpose:

* Fast AI responses
* Lightweight inference
* Cost optimization

---

# 10. Conversation Memory

### Node:

`Simple Memory`

Type:

* Buffer Window Memory

Configuration:

```txt
Context Window Length: 10
```

Purpose:

* Maintains short conversation history
* Provides context-aware replies

---

# 11. Weather Tool Integration

### Node:

`OpenWeatherMap`

Connected as an AI tool.

Capabilities:

* Current weather
* Temperature
* City forecasts

The AI Agent can dynamically call this tool during conversations.

---

# Response Generation

# 12. Response Type Decision

### Node:

`Switch1`

Checks original message type:

* Voice
* Text

---

# Text Reply Flow

### Node:

`Send message`

Sends AI-generated response back as:

* WhatsApp text message

---

# Voice Reply Flow

# 13. AI Voice Generation

### Node:

`Generate audio`

Uses OpenAI Text-to-Speech.

Converts:

* AI text response
  → Voice audio

Output Format:

```txt
opus
```

---

# 14. Audio Format Conversion

### Node:

`Convert Format`

Converts generated audio into:

* WhatsApp-compatible `.ogg opus`

Actions:

* Maintains binary data
* Updates filename
* Sets MIME type correctly

MIME Type:

```txt
audio/ogg; codecs=opus
```

---

# 15. Send Voice Reply

### Node:

`Send message2`

Sends AI-generated voice reply back to the user through WhatsApp.

Message Type:

* Audio

---

# AWS Deployment Advantages

## Scalability

AWS allows scaling workflows for:

* Multiple users
* High message traffic
* Concurrent AI requests

---

## Reliability

* High uptime
* Stable webhook hosting
* Secure API handling

---

## Security

* HTTPS secured endpoints
* Credential management
* API isolation

---

## Performance

AWS enables:

* Faster request handling
* Better response times
* Reliable media processing

---

# Key Features

✅ AWS-hosted automation
✅ Real-time WhatsApp AI assistant
✅ Voice + text support
✅ AI speech recognition
✅ AI-generated voice replies
✅ Context-aware conversations
✅ External API integrations
✅ Tool-calling AI agent
✅ End-to-end automation

---

# Use Cases

This project can be used for:

* AI customer support
* WhatsApp virtual assistant
* Voice-enabled chatbot
* Business automation
* AI helpdesk
* Smart support assistant
* Weather assistant
* Conversational AI system

---

# Future Enhancements

Potential upgrades:

* Multi-user session management
* Database-backed memory
* RAG / vector database integration
* Knowledge base support
* CRM integration
* Voice cloning
* Multi-language conversations
* Analytics dashboard
* Appointment booking
* AI workflow monitoring

---

# Conclusion

This project is a fully automated AI-powered WhatsApp assistant deployed on AWS using n8n.

It combines:

* WhatsApp automation
* Conversational AI
* Voice AI
* Real-time processing
* Cloud scalability

into a production-ready intelligent communication system.
