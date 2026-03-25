# AI Research Agent

A Python-based autonomous research agent powered by Claude (Anthropic) and LangChain. Given a natural language query, the agent reasons about which tools to use, retrieves information from multiple sources, and returns a structured research output — automatically.

---

## What it does

You ask a question. The agent figures out how to answer it.

It searches the web, looks up Wikipedia, synthesizes the results, and saves a structured report — all without manual intervention. The output is always in a consistent, parseable format: topic, summary, sources, and tools used.

**Example:**

```
What can I help you research? > What is the impact of AI on recruitment?

ResearchResponse(
  topic='AI impact on recruitment',
  summary='AI is transforming recruitment by automating screening, reducing bias in CV parsing, and enabling conversational interview agents...',
  sources=['https://...', 'https://...'],
  tools_used=['search', 'wikipedia']
)
```

---

## Architecture

```
User query
    ↓
Agent (Claude claude-3-5-sonnet via LangChain)
    ↓
Tool selection & execution (autonomous)
    ├── Web Search (DuckDuckGo)
    ├── Wikipedia Lookup
    └── File Save (research_output.txt)
    ↓
Structured output (Pydantic model)
    ↓
Parsed ResearchResponse
```

The agent uses a **tool-calling architecture** — the LLM decides which tools to invoke and in what order, based on the query. This is the same pattern used in production AI agent systems.

---

## Key design decisions

**Separation of concerns** — the agent core (`main.py`) and tool definitions (`tools.py`) are kept separate. Adding a new tool means adding it to `tools.py` without touching the agent logic.

**Structured output with Pydantic** — responses are parsed into a typed `ResearchResponse` model, making outputs consistent and programmatically usable rather than free-form text.

**Prompt engineering** — the system prompt instructs the agent to always wrap output in the defined format, with no additional text. This enforces reliable structured responses in production-style usage.

**Claude as the reasoning core** — uses `claude-3-5-sonnet-20241022` via LangChain's Anthropic integration for strong reasoning and instruction-following.

---

## Repository structure

```
AI_Agent/
├── main.py           # Agent definition, prompt engineering, execution loop
├── tools.py          # Tool definitions: search, Wikipedia, file save
├── .env              # API keys (not committed)
├── requirements.txt  # Dependencies
└── README.md
```

---

## Setup

**1. Clone the repo**
```bash
git clone https://github.com/SurabhiDeb/AI_Agent.git
cd AI_Agent
```

**2. Create a virtual environment**
```bash
python -m venv venv
source venv/bin/activate      # macOS/Linux
venv\Scripts\activate         # Windows
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Configure environment variables**

Create a `.env` file in the root directory:
```
ANTHROPIC_API_KEY=your_anthropic_api_key_here
```

---

## Running the agent

```bash
python main.py
```

You will be prompted to enter a research question. The agent will autonomously search, retrieve, and return a structured response. Results are also saved to `research_output.txt`.

---

## Tools

| Tool | Source | Purpose |
|------|--------|---------|
| `search` | DuckDuckGo | Real-time web search |
| `wiki_tool` | Wikipedia API | Encyclopedic background information |
| `save_text_to_file` | Local filesystem | Persists research output with timestamp |

---

## Tech stack

- [LangChain](https://www.langchain.com/) — agent orchestration and tool calling
- [Anthropic Claude](https://www.anthropic.com/) — LLM reasoning core
- [Pydantic](https://docs.pydantic.dev/) — structured output validation
- [DuckDuckGo Search](https://pypi.org/project/duckduckgo-search/) — web search tool
- [Wikipedia API](https://pypi.org/project/wikipedia/) — knowledge retrieval

---

## Planned improvements

- [ ] Multi-agent support (planner + executor pattern)
- [ ] Persistent memory with vector database (e.g. ChromaDB)
- [ ] Tool chaining with intermediate reasoning steps exposed
- [ ] Streamlit UI for non-technical users
- [ ] Logging and monitoring for agent behavior analysis
- [ ] Configurable agent personas and output formats
