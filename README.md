# News Reader Agent

An AI-powered news research and reporting pipeline built with [CrewAI](https://www.crewai.com/). A team of specialized AI agents collaborates to discover, summarize, and curate news articles into a professional daily briefing.

## How It Works

The crew consists of three agents working in sequence:

| Agent | Role | Tools |
|-------|------|-------|
| **News Hunter** | Searches the web for relevant articles, scrapes content, and scores sources by credibility and relevance | Serper search, Playwright scraper |
| **Summarizer** | Produces multi-tier summaries (tweet-length, executive, comprehensive) for each article | Playwright scraper |
| **Curator** | Assembles everything into a polished, publication-ready news briefing with editorial analysis | — |

### Pipeline

```
Search (Serper API) → Scrape (Playwright) → Summarize → Curate → Final Report
```

Output is saved as Markdown files in the `output/` directory:

- `content_harvest.md` — Raw collected articles with metadata and scores
- `summary.md` — Multi-tier summaries for each article
- `final_report.md` — Publication-ready daily news briefing

## Project Structure

```
.
├── main.py              # Crew definition and entrypoint
├── tools.py             # Custom scrape tool (Playwright + BeautifulSoup)
├── config/
│   ├── agents.yaml      # Agent roles, goals, and backstories
│   └── tasks.yaml       # Task descriptions and expected outputs
├── output/              # Generated reports (gitignored)
├── pyproject.toml
└── uv.lock
```

## Prerequisites

- Python 3.13+
- [uv](https://docs.astral.sh/uv/) (recommended) or pip
- API keys:
  - `OPENAI_API_KEY` — used by agents (model: `o4-mini`)
  - `SERPER_API_KEY` — for web search via [Serper](https://serper.dev/)

## Setup

1. **Clone the repository**

   ```bash
   git clone <repo-url>
   cd crewai-agent
   ```

2. **Install dependencies**

   ```bash
   uv sync
   ```

3. **Install Playwright browsers**

   ```bash
   uv run playwright install chromium
   ```

4. **Configure environment variables**

   ```bash
   cp .env.example .env
   ```

   Add your API keys to `.env`:

   ```
   OPENAI_API_KEY=your-openai-key
   SERPER_API_KEY=your-serper-key
   ```

## Usage

Run the crew with a topic of your choice by editing the `topic` input in `main.py`, then:

```bash
uv run python main.py
```

The agents will execute their tasks sequentially and save the results to `output/`.

## Tech Stack

- **[CrewAI](https://www.crewai.com/)** — Multi-agent orchestration framework
- **[Playwright](https://playwright.dev/python/)** — Headless browser for web scraping
- **[BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/)** — HTML content extraction
- **[Serper](https://serper.dev/)** — Google Search API
- **OpenAI o4-mini** — LLM powering all agents
