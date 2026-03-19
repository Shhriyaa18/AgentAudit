# AgentAudit
### Runtime Intent Verification for Multi-Agent AI Systems

AgentAudit is a middleware layer that intercepts every tool call in an AI agent 
system and measures semantic drift from the original user intent before execution 
— blocking out-of-scope actions in real time.

---

## The Problem
AI agents with access to real tools like web search, email, purchasing, and code 
execution can go off task and take irreversible actions the user never intended. 
No existing system intercepts actions before they fire and measures whether they 
align with the original user intent.

## The Solution
AgentAudit embeds the user's original intent using sentence-transformers, then 
before every tool call computes cosine similarity between the intent embedding 
and the proposed action embedding. If drift exceeds the threshold the action is 
blocked and a rollback snapshot is saved.

---

## Live Demo
Run it instantly in Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/shriyasoni/agentaudit/blob/main/AgentAudit.ipynb)

---

## Benchmark Results

| Metric | Score |
|--------|-------|
| Accuracy | 80.0% |
| Precision | 85.7% |
| Recall | 66.7% |
| F1 Score | 75.0% |
| False Positive Rate | 9.1% |

Benchmarked across 20 tasks in 5 categories at optimal threshold 0.45.
Threshold optimized by sweeping 8 values from 0.20 to 0.55.

---

## How It Works
```
User gives a task
       ↓
Intent embedded as 384-dim vector (sentence-transformers)
       ↓
Agent loop starts
       ↓
Agent proposes a tool call
       ↓
AgentAudit intercepts
       ↓
Cosine drift score computed
       ↓
drift > 0.45 → BLOCKED + rollback snapshot saved
drift ≤ 0.45 → ALLOWED + snapshot saved
       ↓
Final answer returned
```

---

## Architecture

- Notebook 1 — Agent loop with 18 tools
- Notebook 2 — Intent embeddings and drift detection
- Notebook 3 — Interceptor middleware and rollback snapshots
- Notebook 4 — Multi-agent chain (Planner → Researcher → Summarizer)
- Notebook 5 — Eval harness and benchmark results
- Final — Combined production notebook with Gradio UI

---

## Tools (18 total)

| Tool | What it does |
|------|-------------|
| web_search | Live web search via Tavily |
| calculator | Any math expression |
| get_weather | Live weather any city |
| currency_converter | Live exchange rates |
| unit_converter | km/miles, kg/lbs, celsius/fahrenheit etc |
| compound_interest | Investment growth calculator |
| loan_calculator | Monthly mortgage payments |
| bmi_calculator | BMI and health category |
| tip_calculator | Restaurant bill splitting |
| age_calculator | Age and birthday countdown |
| calorie_calculator | Calories burned by activity |
| timezone_converter | Time across timezones |
| percentage_calculator | Percentage calculations |
| run_code | Execute Python, SQL, Bash |
| word_counter | Text analysis |
| get_time | Current date and time |
| send_email | Simulated email |
| book_purchase | Simulated purchase |

---

## Tech Stack

- Python
- Groq API (Kimi K2 model)
- sentence-transformers (all-MiniLM-L6-v2)
- Tavily (live web search)
- Gradio (UI)
- scikit-learn (cosine similarity)
- NumPy

---

## Setup

1. Get a free Groq API key at console.groq.com
2. Get a free Tavily API key at app.tavily.com
3. Open the notebook in Google Colab
4. Paste your keys into Cell 2
5. Run Runtime → Restart and run all

---

## Resume Bullets

- Built runtime intent verification middleware intercepting multi-agent tool 
  calls and computing cosine drift from original user intent embedding before 
  execution

- Achieved 85.7% precision catching out-of-scope actions across 20 benchmark 
  tasks at optimal threshold 0.45

- Reduced false positive rate to 9.1% through threshold optimization across 
  8 values (0.20–0.55)

- Implemented rollback snapshot system saving full agent state after every 
  tool call enabling recovery and replay of any execution branch

- Extended to 3-agent chains (Planner → Researcher → Summarizer) with 
  hop-by-hop drift tracking and unified audit log across all agents

- Shipped production Gradio app with chat history, collapsible sidebar, 
  18 tools, and context-aware conversation memory

---

## Author
**Shriya Soni**
[LinkedIn](https://www.linkedin.com/in/shriya-soni-5521b0296/)
