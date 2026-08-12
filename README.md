# Customer Support Multi-Agent System (LangGraph)

An intelligent customer support system built with **LangGraph** that automatically classifies incoming customer queries and routes them to the right specialist agent — with automatic escalation to a human agent when confidence is low or sentiment is negative.

##  Overview

Instead of a single generic chatbot, this system uses a **graph-based multi-agent architecture** where a classifier node analyzes each query's intent, confidence, and sentiment, then routes it via conditional edges to one of four specialized nodes.

##  Features

- **Smart Classification** — Structured output (Pydantic) classifies every query into `billing`, `technical`, `faq`, or `escalation`, along with a confidence score and sentiment.
- **Conditional Routing** — LangGraph's `add_conditional_edges` dynamically routes each query to the correct specialist node based on the classifier's output.
- **Auto-Escalation** — Queries with confidence ≤ 0.6, or containing trigger phrases like "manager", "refund now", "legal", "cancel subscription", are automatically escalated to a human agent.
- **Specialist Agents**:
  -  Billing Agent — payments, invoices, subscriptions, refunds
  -  Technical Agent — bugs, errors, step-by-step troubleshooting
  -  FAQ Agent — general questions
  -  Escalation Agent — hands off to human support
- **Streamlit UI** — Simple web interface to submit queries and view intent, confidence, sentiment, and response in real time.
- **Live Deployment** — Deployed from Google Colab using `pyngrok` for instant public access.

##  Architecture
             ┌─────────────┐
             │  Classifier │
             └──────┬──────┘
                    │ (confidence / intent)
    ┌───────┬───────┼────────┬─────────┐
    ▼       ▼       ▼        ▼

┌────────┐┌────────┐┌─────┐┌────────────┐
│Billing ││Technical││ FAQ ││ Escalation │
└────┬───┘└────┬────┘└──┬──┘└─────┬──────┘
└──────────────END────────────┘


##  Tech Stack

| Component | Technology |
|---|---|
| Agent Orchestration | LangGraph |
| LLM | Groq (Llama 3.3 70B Versatile) |
| Structured Output | Pydantic |
| LLM Integration | LangChain-Groq |
| Frontend | Streamlit |
| Deployment | Pyngrok (public tunnel) |
| Environment | Google Colab |

##  Installation

```bash
pip install -r requirements.txt
```

**requirements.txt**

langgraph
langchain-groq
langchain-core
pydantic
streamlit
pyngrok


##  Setup

1. Get a free API key from [Groq Console](https://console.groq.com).
2. Get a free auth token from [ngrok](https://ngrok.com) (for public deployment).
3. Set them as secrets:
   - In **Google Colab**: use `google.colab.userdata` (Secrets tab)
   - In **local/Streamlit**: create `.streamlit/secrets.toml`:
```toml
     GROQ_API_KEY = "your-groq-api-key"
```

##  Usage

### Run in Google Colab
Open `customer_support_agent.ipynb` and run all cells. The notebook will:
1. Install dependencies
2. Build and compile the LangGraph state machine
3. Run test queries
4. Write `app.py` (Streamlit UI)
5. Launch the app via `pyngrok` and print a public URL

### Run locally
```bash
streamlit run app.py
```

##  Example Queries

| Query | Intent | Escalated |
|---|---|---|
| "How do I reset my password?" | technical |  No |
| "I want a refund NOW or I'm taking legal action!" | escalation |  Yes |
| "hmm okay thanks I guess" | faq (low confidence) | Yes |

##  Project Structure

├── customer_support_agent.ipynb # Main notebook (graph build + Streamlit app writer)
├── app.py # Generated Streamlit application
├── requirements.txt # Python dependencies
└── README.md


##  Future Improvements

- Add conversation memory across multiple turns
- Persist chat history to a database
- Add authentication and multi-user session support
- Replace ngrok with a permanent cloud deployment (Streamlit Cloud / Render)
