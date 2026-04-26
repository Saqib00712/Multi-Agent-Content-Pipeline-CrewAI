# Multi-Agent Content Pipeline with CrewAI
> A collaborative AI system where three specialized agents work together sequentially — a Research Analyst searches the web, a Content Strategist writes a blog post, and a Social Media Strategist crafts platform-ready posts — all powered by CrewAI and Llama 3.2 running locally via Ollama.

---

## What is CrewAI?

CrewAI is a framework for building **multi-agent systems** where each agent has a specific role, goal, and backstory — just like a real team. Agents collaborate by passing their outputs as inputs to the next agent in a sequential pipeline.

```
One agent's output → becomes the next agent's input → final polished result
```

Instead of one LLM doing everything, each agent **specializes** in what it does best — producing significantly better results than a single-agent approach.

---

## Demo

**Input Topic:** *"Latest Generative AI breakthroughs"*

```
[Research Agent]  → searches Google via Serper API
        ↓ detailed research report
[Writer Agent]    → writes a 4-paragraph blog post
        ↓ engaging blog post
[Social Agent]    → creates 2-3 LinkedIn/Twitter posts
        ↓ ready-to-publish social content
```

All three agents run **sequentially and automatically** with a single `crew.kickoff()` call.

---

## Agents

| Agent | Role | Tool | Output |
|-------|------|------|--------|
| **Senior Research Analyst** | Finds cutting-edge insights on any topic | SerperDevTool (Google Search) | Detailed research report |
| **Tech Content Strategist** | Translates research into engaging blog content | None — uses research output | 4-paragraph blog post |
| **Social Media Strategist** | Crafts platform-ready social posts | None — uses blog output | 2-3 LinkedIn/Twitter posts |

---

## Architecture

```
crew.kickoff(inputs={"topic": "..."})
          ↓
[Task 1] Research Task  →  Research Agent
          → SerperDevTool searches Google in real time
          → Returns detailed trend report
          ↓
[Task 2] Writing Task   →  Writer Agent
          → Receives research report as context
          → Returns 4-paragraph blog post
          ↓
[Task 3] Social Task    →  Social Media Agent
          → Receives blog post as context
          → Returns 2-3 social media posts
          ↓
result.raw  →  Final combined output
```

Uses `Process.sequential` — tasks run in order, each building on the previous output automatically.

---

## Tech Stack

![Python](https://img.shields.io/badge/Python-3.11-blue?style=flat-square)
![CrewAI](https://img.shields.io/badge/CrewAI-Multi--Agent-purple?style=flat-square)
![Ollama](https://img.shields.io/badge/Ollama-Llama%203.2-black?style=flat-square)
![Serper](https://img.shields.io/badge/Serper-Google%20Search%20API-orange?style=flat-square)

- **CrewAI** — multi-agent orchestration framework
- **Llama 3.2** via Ollama — local LLM, completely free, no API costs
- **SerperDevTool** — real-time Google Search for the research agent
- **Process.sequential** — tasks chain automatically, output passed between agents
- **Python 3.11** — core language

---

## Project Structure

```
Multi-Agent-Content-Pipeline-CrewAI/
│
├── crew_pipeline.py      # Full pipeline — agents, tasks, crew, output
├── requirements.txt      # All dependencies
├── .env.example          # API key template
└── README.md
```

---

## Getting Started

### 1. Clone the repo
```bash
git clone https://github.com/Saqib00712/Multi-Agent-Content-Pipeline-CrewAI.git
cd Multi-Agent-Content-Pipeline-CrewAI
```

### 2. Install Ollama and pull Llama 3.2 (runs locally — completely free)
```bash
# Download Ollama from https://ollama.com
ollama pull llama3.2
ollama serve    # starts local server on http://localhost:11434
```

### 3. Install Python dependencies
```bash
pip install -r requirements.txt
```

### 4. Set up your Serper API key
```bash
cp .env.example .env
```
Edit `.env` and add your key:
```
SERPER_API_KEY=your_serper_api_key_here
```
Get a free key at [serper.dev](https://serper.dev)

### 5. Run the pipeline
```bash
python crew_pipeline.py
```

### 6. Try any topic
Change this line in `crew_pipeline.py`:
```python
result = crew.kickoff(inputs={"topic": "your topic here"})
```

---

## Key Concepts Covered

- **CrewAI Agent** — defining `role`, `goal`, `backstory`, `tools`, and `llm` for each specialist
- **CrewAI Task** — structured instructions with `description`, `agent`, and `expected_output`
- **Process.sequential** — automatic output chaining between agents
- **allow_delegation** — controlling whether agents can hand off subtasks to each other
- **SerperDevTool** — integrating real-time Google Search into an agent's toolset
- **result.raw** — extracting the final combined output from the crew
- **result.tasks_output** — inspecting each individual agent's output separately
- **Token tracking** — monitoring `prompt_tokens`, `completion_tokens`, `total_tokens`
- **Local LLM via Ollama** — running Llama 3.2 locally with zero API costs

---

## Output Inspection

```python
# Final combined output
print(result.raw)

# Individual agent outputs
print(result.tasks_output[0].raw)   # Research report
print(result.tasks_output[1].raw)   # Blog post
print(result.tasks_output[2].raw)   # Social media posts

# Which agent handled each task
print(result.tasks_output[0].agent) # "Senior Research Analyst"
print(result.tasks_output[1].agent) # "Tech Content Strategist"
print(result.tasks_output[2].agent) # "Social Media Strategist"

# Token usage
print(result.token_usage.total_tokens)
print(result.token_usage.prompt_tokens)
print(result.token_usage.completion_tokens)
```

---

## Example Topics

```python
# Try any of these topics
crew.kickoff(inputs={"topic": "quantum computing breakthroughs of 2024"})
crew.kickoff(inputs={"topic": "LangGraph vs CrewAI comparison 2025"})
crew.kickoff(inputs={"topic": "RAG vs Fine-tuning for LLMs"})
crew.kickoff(inputs={"topic": "OpenAI GPT-5 release and impact"})
crew.kickoff(inputs={"topic": "AI Agents in healthcare 2025"})
```

---

## Why Local LLM (Ollama)?

This project uses **Llama 3.2 locally** instead of OpenAI API — meaning:
- **Zero API costs** — completely free to run
- **Full privacy** — no data sent to external servers
- **Works offline** — no internet needed for the LLM (only Serper for web search)

---

## Related Certifications

Built as part of the IBM **Building AI Agents and Agentic Workflows Specialization** and **Agentic AI with LangGraph, CrewAI, AutoGen and BeeAI** on Coursera.

[![IBM Badge](https://img.shields.io/badge/IBM-AI%20Agents%20Specialization-blue?style=flat-square)](https://www.credly.com/users/muhammad-saqib.361f9b8c)
[![IBM Badge](https://img.shields.io/badge/IBM-CrewAI%20%26%20LangGraph-blue?style=flat-square)](https://www.credly.com/users/muhammad-saqib.361f9b8c)

---

## Author

**Muhammad Saqib**
- GitHub: [@Saqib00712](https://github.com/Saqib00712)
- LinkedIn: [muhammad-saqib](https://www.linkedin.com/in/muhammad-saqib-68b9b3374/)
- Email: saqibkhosa649@gmail.com
- Credly: [15x IBM Certified](https://www.credly.com/users/muhammad-saqib.361f9b8c)
