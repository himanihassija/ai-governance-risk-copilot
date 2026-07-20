# AI Governance & Regulatory Risk Copilot

An autonomous research agent that takes a plain-language description of an AI system and produces a structured **governance and regulatory risk assessment**. The agent identifies applicable regulatory frameworks, classifies risk, outlines compliance obligations, performs a gap analysis, analyzes relevant enforcement precedents, and generates an overall exposure summary. The completed report is automatically saved to a Notion database as structured, formatted blocks.

Built on an **n8n** workflow using a recursive **Deep Research** architecture, the agent continuously generates regulatory search queries, retrieves and analyzes authoritative sources, extracts structured findings, and iteratively expands its research before synthesizing everything into a comprehensive report.

---

## ✨ Features

- 🔍 Accepts a plain-language description of any AI system
- 🌍 Identifies applicable global AI regulations and legal frameworks
- ⚖️ Classifies regulatory risk tiers
- 📋 Maps compliance obligations and required documentation
- 🚨 Performs compliance gap analysis
- 📚 Finds relevant regulatory precedents and enforcement actions
- 📊 Generates an executive exposure summary
- 📝 Automatically writes a structured report into Notion
- 🔄 Recursive deep research with configurable depth and breadth

---

# Workflow Overview

## 1. Intake & Scoping

The workflow begins with a multi-step form where the user describes the AI system to be assessed and specifies configurable **depth** and **breadth** parameters that determine how extensive the research should be.

An LLM then generates clarifying questions regarding:

- Deployment status
- Intended users
- Geographic jurisdictions
- Data handling practices
- Affected stakeholders

These responses help narrow the research scope before execution.

A new report page is simultaneously created inside Notion, and the research process begins asynchronously via a dedicated subworkflow.

---

## 2. Recursive Deep Research Loop

Instead of performing generic web searches, the agent focuses specifically on **regulatory and legal research**.

For every iteration:

1. The LLM generates targeted regulatory search queries.
2. Queries are executed through Apify Web Search.
3. Relevant pages are scraped and converted into Markdown.
4. Another LLM extracts structured learnings such as:
   - Applicable regulations
   - Regulatory authorities
   - Articles and annexes
   - Risk classifications
   - Compliance obligations
   - Documentation requirements
   - Penalties
   - Enforcement dates
   - Monetary fines
5. Newly extracted learnings are accumulated.
6. Based on everything learned so far, the agent generates increasingly specific follow-up queries.

This recursive loop continues until the configured research depth has been satisfied.

---

## 3. Synthesis & Delivery

Once research is complete, every accumulated learning is synthesized into a structured governance report consisting of seven sections:

1. **System Overview**
2. **Applicable Regulatory Frameworks**
3. **Risk Classification**
4. **Compliance Requirements**
5. **Compliance Gap Analysis**
6. **Precedent & Enforcement**
7. **Exposure Summary**

The generated Markdown report is then converted into **Notion's native block schema** using an additional LLM before being uploaded directly to the Notion page.

Finally, the report status is marked as complete.

---

# Tech Stack

| Technology | Purpose |
|------------|---------|
| **n8n** | Workflow orchestration |
| **Groq (Llama Models)** | Clarifying questions, query generation, learning extraction |
| **Mistral** | Markdown → Notion block conversion |
| **Apify** | Web search and webpage scraping |
| **Notion API** | Structured report storage |

---

# Project Architecture

```text
User Input
      │
      ▼
 Intake & Scoping
      │
      ▼
Generate Clarifying Questions
      │
      ▼
Recursive Research Loop
 ├── Generate Queries
 ├── Web Search
 ├── Scrape Pages
 ├── Extract Learnings
 └── Repeat
      │
      ▼
Synthesize Findings
      │
      ▼
Convert Markdown → Notion Blocks
      │
      ▼
Upload Report to Notion
```

---

# Setup

## 1. Import the Workflow

Import `workflow.json` into your n8n instance.

---

## 2. Configure Credentials

Create credentials for the following services and connect them to the corresponding nodes.

### Groq

Used by:

- Groq Chat Model (×5)

---

### Mistral Cloud

Used by:

- Mistral Cloud Chat Model

---

### Apify

Authentication via HTTP Header or Query Authentication.

Used by:

- Web Search
- Page Contents

---

### Notion

Connect your Notion integration to all Notion nodes.

Used by:

- Create Row
- Get Existing Row
- Set In-Progress
- Set Done
- Upload to Notion Page
- Get Existing Row1

---

## 3. Configure Notion

Duplicate the provided Notion database template (or create your own) with the following properties:

- **Name**
- **Description**
- **Request ID**
- **Status**

Share the database with your Notion integration.

---

## 4. Update Database ID

Replace every occurrence of:

```text
YOUR_NOTION_DATABASE_ID
```

with your own Notion database ID.

---

## 5. Activate

Activate the workflow and submit a request through the form trigger.

---

# Example Input

> **"An AI system that analyzes X-rays and MRI scans to help doctors diagnose diseases, used by hospitals across the US, EU, and India."**

---

# Example Output

The generated report includes:

- EU AI Act risk classification
- HIPAA obligations
- EU MDR applicability
- India's DPDP Act requirements
- Required technical documentation
- Human oversight requirements
- Compliance gaps
- Relevant enforcement actions
- Overall regulatory exposure assessment

---

# Scope & Attribution

The recursive research architecture used by this workflow (**search → scrape → extract → iterate**) is adapted from the open-source **DeepResearcher** n8n template.

The AI governance and regulatory risk specialization—including:

- prompt engineering,
- regulatory query generation,
- structured learning extraction,
- report synthesis,
- governance-focused output structure,

was designed and implemented on top of that architecture for this project.

---

# License

This project builds upon an open-source DeepResearcher workflow template while extending it with a specialized AI governance and regulatory risk analysis pipeline.
