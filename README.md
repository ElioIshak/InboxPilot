# InboxPilot - Automated Agent WorkFlow

---
**Project Name:** InboxPilot
**Start Date:** Dec 2025  
**Last Updated:** Jan 2026  
**Author:** Elio (CS student at AUB)

An automated AI-driven email workflow built with n8n that classifies incoming emails and performs appropriate actions such as drafting replies, creating tasks, scheduling meetings, and labeling messages using Gmail, Google Calendar, and Google Tasks.
---

## Overview

This project implements an event-driven automation pipeline triggered by new Gmail messages.  
Incoming emails are analyzed using a language model to determine their intent, after which deterministic rules route the email to the correct action.

The workflow is designed to safely combine AI-based reasoning with explicit control logic.
---

## Workflow Diagram

The diagram below represents the full workflow exactly as implemented in n8n.
![Automated Agent Workflow](https://github.com/ElioIshak/Automated-Email-Triage-Agent/blob/main/screenshots/Automated_Agent_WorkFlow.PNG)
---

## Workflow Description

### 1. Gmail Trigger
- Listens for new incoming emails
- Acts as the entry point of the workflow

### 2. Initial Filtering
- An `If` node checks for newsletter, unsubscribe, or no-reply patterns
- Filtered emails are labeled as junk and exit the workflow

### 3. AI Analysis
- Remaining emails are sent to a language model
- The model returns a **strict JSON response** containing:
  - Email category (Reply, Meeting, Task, FYI, Spam)
  - Urgency
  - Summary
  - Suggested reply
  - Task or meeting details (if applicable)

### 4. Category Routing
- A `Switch` node routes execution based on the predicted category

### 5. Actions
Depending on the category, the workflow performs one or more of the following:

- **Reply**
  - Creates a Gmail draft using the suggested reply

- **Task**
  - Creates a Google Task with title, due date, and notes

- **Meeting**
  - Splits proposed time options
  - Iterates over options and checks calendar availability
  - Creates a calendar event if no conflicts are found
  - If all options conflict, creates a fallback Gmail draft

- **FYI**
  - Applies informational labels and removes the inbox label

- **Spam / Spamish**
  - Applies AI and spam-related Gmail labels
---

## Labels Used

- `AI`
- `FYI`
- `SPAMISH`
- `Junk`

These labels are applied automatically based on workflow decisions.
---

## Tech Stack

- n8n (workflow orchestration)
- OpenAI API (email intent analysis)
- Gmail API
- Google Calendar API
- Google Tasks API
---

## Design Notes

- The language model is used **only for decision-making**, not execution
- All side effects (drafts, tasks, events, labels) are controlled by deterministic nodes
- Emails are drafted, not sent automatically, to ensure safety
- Meeting conflicts are explicitly checked before event creation
---

## Status

This workflow is complete and functional and is intended for controlled personal productivity automation and experimentation.
