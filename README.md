## Overview

`conversational_knowledge_system` is an **AI-assisted conversational knowledge extraction system**. It allows users to chat with an AI agent to clarify a topic, then explicitly **save** the conversation into structured, long-term memory using a **predefined or AI-assisted template**.

The system is designed with a **human-in-the-loop** approach: the user controls when information is persisted, while the AI agent is responsible for extracting, structuring, and migrating knowledge in a reliable and schema-driven way.

This project focuses on **AI system engineering**, not just chat functionality.

---

## Key Features

* 💬 **Conversational clarification**: Multi-turn chat to explore and refine ideas
* 🧠 **AI-assisted template design**: The agent proposes multiple template options for a topic; the user selects and locks one
* 📐 **Schema-driven extraction**: Important information is extracted into structured JSON that strictly follows the selected template
* 🧩 **Flexible templates**:

  * Fixed-field templates (e.g. meetings, research planning)
  * Dynamic-key templates (e.g. knowledge notes, where concepts become keys)
* 🔄 **Template versioning & migration**: When a template changes, existing data is migrated into a new version without data loss
* 📁 **Topic-based storage**: Each topic maintains its own templates, versions, and extracted data
* 📄 **PDF export**: Structured knowledge can be exported to a formatted PDF for long-term use

---

## Motivation

In many real-world scenarios (research, learning, planning), valuable information emerges gradually through conversation. However:

* Important insights are easily lost in chat history
* Manual note-taking interrupts thinking
* Rigid note templates do not adapt well to evolving topics

This project explores how **LLM-based agents** can be used to bridge conversational interaction and structured knowledge storage, while maintaining **user control, reliability, and explainability**.

---

## System Architecture

```
User
 ↓
Chat API
 ↓
Conversation Store
 ↓ (Save Trigger)
Extraction Agent
 ↓
Schema Validator
 ↓
Topic Folder (Versioned Templates & Data)
 ↓
Markdown → PDF Export
```

Key design principle:

> *LLMs are treated as reasoning components inside a deterministic system, not as the system itself.*

---

## Template System

### 1. Fixed-Field Templates

Used for structured scenarios such as meetings or project planning.

Example:

```json
{
  "goals": [],
  "decisions": [],
  "action_items": [],
  "deadlines": []
}
```

### 2. Dynamic-Key Templates

Used for exploratory knowledge capture, where the AI determines the keys (e.g. knowledge points), but values follow a fixed schema.

Example:

```json
{
  "knowledge_points": {
    "Backpropagation": {
      "summary": "...",
      "examples": "..."
    }
  }
}
```

This hybrid approach provides flexibility without sacrificing structure or validation.

---

## Template Lifecycle

1. **Topic creation** – User defines a topic
2. **Template proposal** – AI suggests multiple template options
3. **Template refinement** – User discusses and adjusts via chat
4. **Template locking** – Selected template is frozen as version `v1`
5. **Extraction on save** – Conversations are extracted into structured data
6. **Template update (optional)** – New template version is created (`v2`)
7. **AI-assisted migration** – Existing data is migrated to the new schema

Older versions are preserved for traceability.

---

## Agent Responsibilities

The AI agent is responsible for:

* Identifying important information from multi-turn conversations
* Mapping free-form dialogue to structured schema fields
* Normalizing dynamic keys (e.g. concept names)
* Producing schema-compliant JSON outputs
* Assisting with template migration between versions

All persistence actions are **explicitly user-triggered**.

---

## PDF Export

Stored structured data can be exported to PDF via the following pipeline:

```
Structured JSON → Markdown → PDF
```

The export format is template-aware, enabling clean, readable documents for study notes, research planning, or documentation.

---

## Tech Stack

* **Backend**: FastAPI
* **Language**: Python
* **LLM Integration**: Structured output / tool-calling
* **Storage**: File-based JSON (topic folders)
* **Validation**: JSON Schema
* **Export**: Markdown → PDF

---

## Project Scope & Design Choices

* Single-user system (no authentication)
* Manual save trigger (human-in-the-loop)
* No vector database or retrieval augmentation
* Focus on correctness, traceability, and system design

These choices are intentional to emphasize **AI engineering discipline over feature bloat**.

---

## Future Improvements (Out of Scope for MVP)

* Search across topics
* Multi-user support
* UI frontend
* Notification / reminder system

---

## Author

Jiang Chen
Master of Computing (Artificial Intelligence), NUS
Software Engineer (Backend & AI Systems)

---

## License

MIT License
