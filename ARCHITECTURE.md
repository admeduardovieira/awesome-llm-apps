# Architecture Analysis: awesome-llm-apps

## Repository Overview

**awesome-llm-apps** is a comprehensive collection of 100+ open-source AI agents, agent skills, and RAG applications built with various LLM frameworks and tools.

### Repository Structure

```
awesome-llm-apps/
├── agent_skills/                      # Agent Skills (drop-in for coding agents)
│   ├── project-graveyard/             # Dead project autopsy tool
│   ├── first-reader/                  # Beta reader simulation
│   ├── scope-creep-detector/          # Diff analysis for scope creep
│   ├── commit-archaeologist/          # Git history analysis
│   ├── dependency-doctor/             # Dependency manifest health check
│   ├── advisor-orchestrator-worker/   # Multi-model orchestration
│   ├── thinking-out-loud/             # Voice ramble analysis
│   ├── self-improving-agent-skills/   # Gemini/ADK based skill optimizer
│   ├── registry.json                  # Machine-readable skill registry
│   ├── README.md
│   └── evals/                         # Deterministic tests for each skill
├── starter_ai_agents/                 # Single-file agents (API key required)
├── advanced_ai_agents/                # Production-grade agents
├── always_on_agents/                  # Scheduled/background agents
├── mcp_ai_agents/                     # Model Context Protocol agents
├── rag_tutorials/                     # RAG implementations
├── voice_ai_agents/                   # Voice-enabled agents
├── generative_ui_agents/              # Agents with UI rendering
├── advanced_llm_apps/                 # Browser tools, optimization tools
├── ai_agent_framework_crash_course/   # Tutorials (ADK, OpenAI SDK)
├── .github/workflows/                 # CI/CD pipelines
│   ├── claude.yml                     # Claude PR assistant
│   └── skill-evals.yml                # Skill eval pipeline
└── README.md
```

## Entry Points & Frameworks

### Primary Frameworks Used

1. **Agno** (v2.0+) - Primary agent framework
   - Dependencies: `agno>=2.2.10`, `agno>=2.3.24`
   - Used in: travel agents, financial agents, news/podcast agents

2. **OpenAI Agents SDK** - Official OpenAI framework
   - Used in: Crash Courses, research agents

3. **Google ADK (Agent Development Kit)** - Google's agent framework
   - Used in: Self-improving agent skills, research agents

4. **AG2** - Agent swarm framework
   - Used in: Adaptive research team

5. **CrewAI** - For multi-agent workflows
   - Used in: AI Services Agency

6. **EvoAgentX** - For self-evolving agents
   - Used in: AI Self-Evolving Agent

### LLM Providers

| Provider | Models Used | Location |
|----------|-------------|----------|
| OpenAI | GPT-4o, GPT-4o-mini-tts | Multiple agents |
| Google | Gemini 3.8 Flash, Gemini 3.1 Pro | Self-improving skills, some agents |
| xAI | Grok | xAI Finance Agent |
| DeepSeek | DeepSeek R1 | AI System Architect, 3D Pygame Agent |
| Ollama | Llama 3.2, OpenHermes | Local agents, RAG |
| Qwen | Various | Some multilingual agents |

## Infrastructure & Dependencies

### Banned Patterns Detection (skill_scanner.py)

The repo includes a security scanner that detects:
- Remote script execution (`curl | sh`, `wget | sh`)
- Base64 obfuscation
- Credential access patterns
- Network call patterns without declaration

### Vector Databases

1. **Qdrant** - Local RAG agents, memory implementations
2. **Unknown** - Some agents may use proprietary databases

### API Integrations

| Service | Usage |
|---------|-------|
| SerpAPI | Web search (travel agents) |
| Firecrawl | Web scraping, research |
| Exa | Search |
| Google Maps/Airbnb APIs | Travel planning |
| OpenWeatherMap | Weather data |
| NewsAPI | News aggregation |
| Stripe | Payment integration (auth tests) |
| GitHub API | Repository analysis |
| YouTube API | Video transcripts |

## Agent Architecture Patterns

### 1. Single-Agent Pattern
- Minimal code, direct LLM calls
- Example: `starter_ai_agents/ai_travel_agent/travel_agent.py`
- Uses Agno Agent with OpenAI model

### 2. Multi-Agent Team Pattern
- Multiple specialized agents collaborate
- Example: `ai_travel_planner_agent_team/backend/`
- Agents: Destination Explorer, Hotel Search, Dining Agent, Budget Agent, Flight Agent, Itinerary Specialist

### 3. Tool-Augmented Agents
- Agents with built-in/external tools
- SerpAPI for web search
- File system tools for document access

### 4. RAG Pattern
- Retrieval + Generation pipeline
- Qdrant for vector storage
- Agno Knowledge class for document management

## Environment Variables

### Common Variables

```bash
OPENAI_API_KEY=your_openai_key
GOOGLE_API_KEY=your_google_key
GEMINI_MODEL=gemini-3.8-flash
SERPAPI_KEY=your_serpapi_key
EXA_API_KEY=your_exa_key
FIRECRAWL_API_KEY=your_firecrawl_key
OLLAMA_HOST=http://localhost:11434
```

### Project-Specific Variable Files

- `agent_skills/project-graveyard/env.example` - None (offline tool)
- `advanced_ai_agents/multi_agent_apps/ai_travel_planner_agent_team/backend/.env.example`
- `rag_tutorials/local_rag_agent/env.example` - None (fully local)

## Test Infrastructure

### Evals System (agent_skills/evals/)

**Tier 1 - Structural**
- `skill_lint.py` - Validates SKILL.md spec compliance
- Checks: naming, description, file references, line limits

**Tier 1b - Security**
- `skill_scanner.py` - OWASP AST01-AST10 pattern detection
- Detects: install lures, network calls, credential access, obfuscation

**Tier 2 - Trigger & Routing**
- `run_trigger_evals.py` - Positive/negative trigger cases
- `test_registry_lint.py` - Unit tests for registry validation

**Tier 2b - Deterministic Scripts**
- `<skill>/test_*.py` - Executable tests with synthetic fixtures
- Example: `project-graveyard/test_graveyard.py` (16 checks)

**Tier 3 - Behavioral**
- Agent session testing with expectations
- Uses skill-creator eval schema

## CI/CD Pipeline

### skill-evals.yml

```yaml
jobs:
  evals:
    # Runs all test_*.py files in agent_skills/evals/*/
  lint:
    # Runs skill_lint.py --strict on all skills
  security-scan:
    # Runs skill_scanner.py on agent_skills
  trigger-routing:
    # Validates trigger cases
  registry:
    # Validates registry.json and README listings
```

## Execution Flow

### Skill Installation Flow
1. User runs: `npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/<skill>`
2. Skills CLI detects compatible agent
3. Skill folder copied to agent's skills directory
4. Agent discovers skill via SKILL.md frontmatter
5. User invokes skill with natural language prompt
6. Agent runs bundled scripts with provided arguments

### Starter Agent Flow
1. `pip install -r requirements.txt`
2. Set up environment variables (.env file)
3. Run: `streamlit run <agent>.py` or `python <agent>.py`
4. Interact via web UI or CLI

### RAG Agent Flow
1. Start vector DB (Qdrant): `docker run -p 6333:6333 qdrant/qdrant`
2. Install model locally: `ollama pull llama3.2`
3. Run: `python local_rag_agent.py`
4. Access via AgentOS web interface (port 7777)

## Security Model

- **Local-first**: Most skills run entirely offline
- **Read-only by default**: Scanners don't modify scanned repos
- **Explicit network declarations**: Required in SKILL.md compatibility field
- **Supply chain protection**: CI scans for injection patterns
- **No curl|bash**: Prohibited in all documentation

## Key Architectural Decisions

1. **SKILL.md Standard**: Frontmatter-based skill discovery with strict spec
2. **Deterministic Evals**: All skills must have passing tests before CI merge
3. **Security-First**: Dedicated security scanner for supply-chain threats
4. **Multi-Framework Support**: Skills work across Claude Code, Codex, Cursor, Hermes, etc.
5. **Offline by Default**: Minimal network dependencies, explicit declarations