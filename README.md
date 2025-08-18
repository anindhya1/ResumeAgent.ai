# ResumeAgent.ai

An AI-powered agent that answers questions about my resume in real time — like a personal career assistant you can chat with.

**Live demo:** https://huggingface.co/spaces/Anindhya/ResumeAgent.ai

---

## What it does

- **Conversational Q&A** – ask about experience, projects, skills, education, and get precise answers instantly  
- **Context-grounded answers** – responses are based on `me/summary.txt` and `me/linkedin.pdf`  
- **Smart tools via function calling** –  
  - `record_user_details(email, name, notes)` → records interest for follow-up  
  - `record_unknown_question(question)` → logs questions the agent could not answer  
- **Evaluator schemas (Pydantic)** – optional self-evaluator utilities for stricter validation and retries  
- **Clean UI** – Gradio chat interface for simple, interactive use  
- **Notifications** – optional Pushover integration for alerts when someone shares contact info or asks an unknown question  

---

## How it works (architecture)

1. **Context loading**  
   - Loads `summary.txt` and extracts text from `linkedin.pdf` (via `pypdf`) into the system prompt.  

2. **LLM chat loop**  
   - Uses OpenAI’s `gpt-4o-mini` for dialogue, with function-calling tools enabled.  
   - If the model decides a tool is needed, it issues a structured function call which is then executed.  

3. **Tool execution**  
   - Tools are implemented with clear schemas and executed based on LLM calls.  
   - Outputs are validated and passed back to the chat loop.  

4. **Evaluator (optional)**  
   - Includes Pydantic-based evaluator schema for structured feedback and retry loops.  
   - Can be enabled if stricter guardrails are needed.  

---

## Project layout

- `app.py` — main Gradio app, chat loop, tools, evaluator helpers  
- `me/summary.txt` — narrative summary of background (required)  
- `me/linkedin.pdf` — exported LinkedIn profile PDF (required)  
- `README.md` — documentation  

> Ensure the `me/` folder exists with both files before running.

---

## Setup

**Requirements**
- Python 3.10+ recommended  

**Install**
    
    git clone https://github.com/<your-username>/ResumeAgent.ai.git
    cd ResumeAgent.ai
    pip install -U python-dotenv openai gradio pypdf pydantic requests

**Environment**

Create a `.env` file:

    # Required
    OPENAI_API_KEY=sk-your-key

    # Optional (for notifications)
    PUSHOVER_TOKEN=your-pushover-app-token
    PUSHOVER_USER=your-pushover-user-key

**Run locally**

    python app.py

App will be available at `http://127.0.0.1:7860/`.

---

## Example questions

- "What’s your experience with NLP and knowledge graphs?"  
- "Tell me about a project you’re most proud of."  
- "What tools and frameworks do you use regularly?"  
- "How can I contact you about an opportunity?"  

---

## Configuration tips

- **Model choice** – swap `gpt-4o-mini` in `app.py` with another model string if desired  
- **Notifications** – currently uses Pushover; can be swapped for email/webhooks in `push()`  
- **Context sources** – update `summary.txt` and `linkedin.pdf` for best results  

---

## Deploy to Hugging Face Spaces

This repo is ready for Spaces (Gradio). To enable:

    ---
    title: ResumeAgent.ai
    app_file: app.py
    sdk: gradio
    sdk_version: 5.34.2
    ---

Push to your Hugging Face Space, and it will boot automatically.

---

## Concepts Implemented

- **Pydantic** – structured validation for evaluator schemas and tool inputs/outputs  
- **Function calling** – LLM triggers tools with structured arguments  
- **Grounded context** – uses personal résumé files for accurate responses  
- **Environment-based config** – `.env` keeps keys and secrets out of code  
- **Notifications** – optional Pushover alerts for leads and unknown questions  
- **Extensible design** – tools and evaluators can be extended or replaced  

---

## Privacy

- Only reads files in `me/`  
- Email addresses and unknown questions are not stored in the repo (only sent as notifications if enabled)  
- No tracking beyond your chosen LLM provider  

---

## Roadmap

- Vector store for larger résumés/portfolios  
- Source citations in answers  
- Additional notification channels (email, Discord, Slack)  
- Built-in evaluator toggle in the UI  

---

## License

MIT License  

---

## Acknowledgments

- [Gradio](https://gradio.app/) – chat interface  
- [OpenAI Python SDK](https://github.com/openai/openai-python) – LLM backend  
- [pypdf](https://pypi.org/project/pypdf/) – PDF text extraction  
- [Pydantic](https://docs.pydantic.dev/) – structured validation  
- [Requests](https://requests.readthedocs.io/) – lightweight API calls  
