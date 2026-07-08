# Trust-Audit Co-Author Agent

<div align="center">

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Gemini API](https://img.shields.io/badge/Gemini-2.5_Flash-orange.svg)](https://ai.google.dev/)
[![Pydantic](https://img.shields.io/badge/Pydantic-v2-red.svg)](https://docs.pydantic.dev/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626.svg)](https://jupyter.org/)

An advanced AI workflow agent that merges rigorous academic critique with structured document co-authoring.
</div>

---

## 📖 Overview

Most LLM-based academic tools are designed simply to summarize research papers. The **Trust-Audit Co-Author Agent** takes a radically different approach by acting as a critical peer reviewer and drafting assistant. 

It executes a two-phase pipeline:
1. **The Audit Phase:** Critically evaluates research abstracts or paper excerpts, actively flagging methodological flaws (e.g., selection bias, missing control groups, unstated confidence intervals, funding conflicts) and generating a standardized Trust Score.
2. **The Co-Author Phase:** Utilizes the generated critique to automatically draft structured, professional documents such as Literature Reviews, Peer Review Reports, or Rebuttals.

This project is built around strict Pydantic schemas ensuring deterministic JSON outputs, coupled with built-in token telemetry and cost estimation.

## ✨ Core Capabilities

- **Methodological Flagging:** Autonomously detects 10+ common research flaws (Correlation vs. Causation, Small Sample Sizes, Survivorship Bias, etc.).
- **Quantitative Trust Scoring:** Generates a plain-language credibility score (1-10) backed by clear, beginner-friendly reasoning.
- **Context-Aware Drafting:** Drafts actionable, highly specific document sections based *only* on the provided context (zero hallucination tolerance).
- **Cost Telemetry:** Real-time tracking of input/output token usage mapped to current API pricing models.
- **Deterministic Output:** Enforces strict response schemas using `Pydantic` and Google GenAI's structured output controls.

## 🏗️ Architecture

```text
Field_work_task_1/
├── DA.ipynb            # Core Jupyter Notebook containing the agent logic
├── prompt.yml          # Externalized system prompt dictating behavior & constraints
├── requirements.txt    # Python dependency specifications
├── .env                # Environment variables (Ignored in version control)
└── .gitignore          # Standard Python & Jupyter ignore rules
```

## 🚀 Getting Started

### Prerequisites
- Python 3.10 or higher
- A valid Google Gemini API Key

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/AZMSharif/Field_work_task_1.git
   cd Field_work_task_1
   ```

2. **Establish a virtual environment (Recommended):**
   ```bash
   # Windows
   python -m venv venv
   .\venv\Scripts\activate
   
   # macOS/Linux
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables:**
   Create a `.env` file in the project root and add your API key:
   ```env
   GEMINI_API_KEY=your_google_gemini_api_key_here
   ```

### Execution

Launch the Jupyter environment to interact with the agent:
```bash
jupyter notebook DA.ipynb
```

## 💻 Usage Example

**Input Payload:**
> "Audit this abstract and draft a critique for my lit review: 'A study of 12 participants showed that drinking green tea daily reduced anxiety by 40%. The study was funded by GreenTea Corp and had no control group. Participants were self-selected volunteers from a wellness forum.'"

**Agent Output (Parsed via Pydantic):**
```text
TASK TYPE       : full_audit_and_draft
TRUST SCORE     : 2/10 — Low Trust
DOCUMENT TYPE   : Literature Review section

WEAKNESSES FLAGGED:
  - Small sample size (N=12)
  - No control group or missing baseline comparison
  - Funding conflict of interest (GreenTea Corp)
  - Selection bias (wellness forum volunteers)

AUDIT REASONING:
  This study possesses critical methodological flaws that severely undermine its conclusions. The tiny sample size, lack of a control group, and obvious funding bias make the 40% claim statistically unreliable.

DRAFTED CRITIQUE:
  - While one study reported a 40% reduction in anxiety linked to green tea, these findings should be interpreted with extreme caution due to severe methodological limitations.
  - The sample size (N=12) is insufficient for broad generalization, and the reliance on self-selected volunteers from a wellness forum introduces significant selection bias.
  - Furthermore, the absence of a control group makes it impossible to isolate the effect of green tea from placebo or other environmental factors.
  - Notably, the study was funded by GreenTea Corp, representing a direct conflict of interest that compromises the perceived objectivity of the research.

WRITING TIPS:
  - Ensure your tone remains objective and academic, focusing on the methodology rather than the researchers themselves.

FOLLOW-UP ACTIONS:
  - Search for larger, peer-reviewed, double-blind studies on green tea and anxiety.
```

## 📊 Telemetry & Cost Tracking

The agent wraps API calls in a telemetry function that intercepts `usage_metadata`. Costs are calculated dynamically based on predefined constants for the target model (`gemini-2.5-flash`).

```python
# Example Terminal Output
TOKENS  : 480 in + 609 out tokens = ~$0.0004
```

## 🛡️ Security Note

The `.env` file containing your API keys is explicitly ignored via `.gitignore`. Never commit API keys or sensitive credentials to version control.
