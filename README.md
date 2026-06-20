# Ticket Router

A robust Python-based routing engine that parses unstructured, raw customer support complaints and converts them into strictly typed Python objects using Pydantic and Large Language Models (LLMs). This project is designed to act as a middleware component that traditional backends can query to route tickets to the appropriate department.

## Features

- **Guaranteed Schema Adherence**: Uses OpenAI's Structured Outputs API (routed through OpenRouter) to ensure LLM responses match our exact Pydantic schema structure.
- **Dynamic Classification**: Automatically categorizes customer issues into Billing, Technical Support, Refund Request, or Spam/Irrelevant.
- **Urgency Scoring**: Computes a priority urgency score (1-10) to identify critical or system-down events.
- **Summarization**: Generates a concise, 5-word summary of customer complaints for dashboard views.
- **Robust Validation**: Enforces data validation and type safety using Pydantic v2.

---

## Technical Architecture

The core of the application revolves around:
1. **`SupportTicket` Pydantic Model**: Defines the target schema (`category`, `urgency_score`, `summary`).
2. **OpenAI SDK / OpenRouter API**: Integrates with LLMs (specifically `openai/gpt-4o-mini`) using the `client.beta.chat.completions.parse` method.
3. **Environment Management**: Uses `python-dotenv` to safely load configuration and keys.

---

## Prerequisites

- Python 3.10 or higher
- An OpenRouter API Key (or OpenAI API Key)

---

## Installation & Setup

Follow these steps to set up the project locally:

### 1. Clone & Navigate
```bash
git clone <repository-url>
cd ticket-router
```

### 2. Set Up Virtual Environment
Create and activate a virtual environment to manage dependencies:

**On Windows:**
```powershell
python -m venv .venv
.\.venv\Scripts\activate
```

**On macOS/Linux:**
```bash
python -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies
Install all required packages from `requirements.txt`:
```bash
pip install -r requirements.txt
```

### 4. Configuration
1. Copy the example environment template:
   ```bash
   cp .env.example .env
   ```
2. Open the new `.env` file and insert your credentials and configuration:
   ```env
   # Your OpenRouter or OpenAI API Key
   OPENAI_API_KEY=your_openrouter_api_key_here

   # The model to run classification on (defaults to openai/gpt-4o-mini)
   MODEL_NAME=openai/gpt-4o-mini
   ```

   > [!TIP]
   > You can change `MODEL_NAME` to any model offered by OpenRouter (e.g., `google/gemini-2.5-flash`, `anthropic/claude-3.5-haiku`, or `meta-llama/llama-3.3-70b-instruct`). Ensure the model you choose supports Structured Outputs (JSON Schema parsing) for best results. Refer to the comments in `[.env.example](file:///c:/Users/Asimr/3D%20Objects/Projects/ticket-router/.env.example)` for details.

---

## Running the Application

You can execute the demonstration script containing sample complaints to see the router in action:

```bash
python app.py
```
