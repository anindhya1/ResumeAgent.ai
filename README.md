# ResumeAgent.ai

An AI-powered agent that answers questions about my resume in real time — like a personal career assistant you can chat with.

**Live demo:** https://huggingface.co/spaces/Anindhya/ResumeAgent.ai

---

## What it does

- **Conversational Q&A:** Ask about experience, projects, skills, education — get precise answers instantly.
- **Understands my profile:** The agent loads a *summary* and my *LinkedIn PDF* for grounded, accurate replies.
- **Smart actions via tools:**  
  - Records interest with email + notes (for follow-ups).  
  - Logs unanswered questions (so I can improve coverage).
- **Clean UI:** Built with Gradio’s chat interface for a simple, fast experience.

---

## How it works (architecture)

1. **Context loading**
   - Reads `me/summary.txt` and text-extracts `me/linkedin.pdf` (via `pypdf`) into the system prompt.

2. **LLM chat loop**
   - Uses OpenAI’s `gpt-4o-mini` to chat, with function-calling tools enabled.
   - If the model asks to call a tool, the app executes it and continues the conversation.

3. **Tools (function calling)**
   - `record_user_details(email, name, notes)` → sends a **Pushover** notification so I can follow up.
   - `record_unknown_question(question)` → sends a Pushover note with the missed question.

4. **(Optional) self-evaluator utilities**
   - Includes a Pydantic-based evaluator schema prepared for quality checks and automatic retrying with feedback (helper methods in code).  
     *Note:* The main chat flow focuses on fast responses; evaluator helpers are available if you want stricter guardrails.

---

## Project layout

- `app.py` — main Gradio app + agent logic, OpenAI calls, tools, and (optional) evaluator helpers  
- `me/summary.txt` — short narrative summary of my background (required)  
- `me/linkedin.pdf` — exported LinkedIn profile PDF used for grounded answers (required)  
- `README.md` — this file

> Make sure the `me/` folder exists and contains both files.

---

## Setup

**Requirements**
- Python 3.10+ recommended

**Install**
    
    git clone https://github.com/<your-username>/ResumeAgent.ai.git
    cd ResumeAgent.ai
    pip install -U python-dotenv openai gradio pypdf pydantic requests

**Environment**

Create a `.env` file in the project root:

    # Required
    OPENAI_API_KEY=sk-your-key

    # Optional (for notifications when someone leaves their email or asks an unknown question)
    PUSHOVER_TOKEN=your-pushover-app-token
    PUSHOVER_USER=your-pushover-user-key

> If you don’t want notifications, you can leave the Pushover variables unset or stub the `push()` function in `app.py`.

**Run locally**

    python app.py

Then open the local URL printed in the terminal (usually `http://127.0.0.1:7860/`).

---

## Try these example questions

- “What’s your experience with NLP and knowledge graphs?”  
- “Tell me about a project you’re most proud of.”  
- “What tools and frameworks do you use regularly?”  
- “How can I contact you about an opportunity?” *(the agent will politely ask for an email and record it)*

---

## Configuration tips

- **Model choice:** In `app.py`, search for `gpt-4o-mini` and swap with another model string if needed.
- **Notification stack:** Using Pushover for simplicity; swap out with email/webhook/etc. by editing `push()` and the two tool functions.
- **Context sources:** Replace `me/summary.txt` and `me/linkedin.pdf` with your own files — keep them succinct and up-to-date for best results.

---

## Deploy to Hugging Face Spaces

This repo is ready for Spaces (Gradio). If you use Spaces’ README metadata, include this YAML at the **top** of the Space’s README (not necessary on GitHub):

    ---
    title: ResumeAgent.ai
    app_file: app.py
    sdk: gradio
    sdk_version: 5.34.2
    ---

Then push your repo to a Space and it should boot automatically.

---

## Privacy

- The agent only reads the files you provide in `me/`.  
- Email addresses and unknown questions are **not stored in the repo** — they’re sent as notifications (if Pushover is configured).  
- No analytics or third-party tracking beyond your chosen LLM provider.

---

## Roadmap

- Optional vector store for larger résumés/portfolios  
- Source citations in answers  
- More channels for follow-up (email/Discord/webhooks)  
- Built-in evaluator loop toggle in the UI

---

## License

MIT — see `LICENSE` (or choose your preferred license).

---

## Acknowledgments

Built with:
- [Gradio](https://gradio.app/)
- [OpenAI Python SDK](https://github.com/openai/openai-python)
- [pypdf](https://pypi.org/project/pypdf/)
- [Pydantic](https://docs.pydantic.dev/)
- [Requests](https://requests.readthedocs.io/)

**Live demo:** https://huggingface.co/spaces/Anindhya/ResumeAgent.ai
