# Pull Requests Sugeridos

## PR #1: Corrigir Inconsistência de Gerenciador de Dependências

**Título:** `fix(ai-travel-planner): update README to use uv instead of pip requirements.txt`

**Arquivos a modificar:**
- `advanced_ai_agents/multi_agent_apps/agent_teams/ai_travel_planner_agent_team/README.md`

**Mudança proposta:**

```diff
## 🛠️ Tech Stack

-**Frontend:** Next.js, React, TypeScript
-**Backend:** Python, FastAPI, PostgreSQL
-**AI:** Agno (agent coordination), Gemini (LLM), Exa (search), Firecrawl (web scraping)
-**APIs:** Google Flights, Kayak
+**Frontend:** Next.js, React, TypeScript
+**Backend:** Python, FastAPI, PostgreSQL
+**AI:** Agno (agent coordination), Gemini (LLM), Exa (search), Firecrawl (web scraping)
+**APIs:** Google Flights, Kayak
+**Package Manager:** uv (ultra-rápido, recomendado)

## 📥 Installation

### Quick Start

```bash
cd backend

# Opcional: instalar uv (muito mais rápido que pip)
pip install uv

# Instalar dependências
uv pip install pyproject.toml

# Ou tradicionalmente:
pip install -r <(python -c "import tomllib; d=tomllib.load(open('pyproject.toml','rb')); print('\\n'.join(d['project']['dependencies']))")
```

## 🏃 Como Executar

```bash
# Iniciar o servidor
python main.py  # ou: uv run python main.py

# Ou via docker
./docker.sh
```
```

**Motivo:** Bug CRÍTICA - Usuário tenta seguir instruções do README e falha porque `requirements.txt` não existe. O projeto usa uv com `uv.lock`.

**Benefício:** Usuários conseguem instalar e rodar o agente sem confusão.

---

## PR #2: Adicionar Template de Variáveis de Ambiente

**Título:** `docs(self-improving-agent-skills): add .env.example template file`

**Arquivos a modificar:**
- `agent_skills/self-improving-agent-skills/backend/.env.example` (novo arquivo)

**Conteúdo do novo arquivo:**

```bash
# ============================================
# Google AI Studio API Configuration
# ============================================
# Get your API key from: https://aistudio.google.com/apikey
GOOGLE_API_KEY=your_google_api_key_here

# ============================================
# Model Configuration (optional)
# ============================================
# Default: gemini-3.8-flash
# Other options: gemini-3.1-pro-preview, gemini-2.0-flash
GEMINI_MODEL=gemini-3.8-flash

# ============================================
# Server Configuration (optional)
# ============================================
# Host to run the server on
HOST=0.0.0.0

# Port to run the server on
PORT=8891
```

**Título alternativo:** `fix(self-improving-agent-skills): create .env.example for environment setup`

**Motivo:** Bug MÉDIA - Usuário tenta `cp .env.example .env` e falha porque o arquivo não existe.

**Benefício:** Users podem configurar o ambiente corretamente sem adivinhar quais variáveis são necessárias.

---

## PR #3: Documentar Suporte a Ollama

**Título:** `docs(ai-travel-agent): add Ollama installation prerequisites for local mode`

**Arquivos a modificar:**
- `starter_ai_agents/ai_travel_agent/README.md`

**Mudança proposta:**

```diff
## 🌐 Local vs Cloud Version

-**- travel_agent.py**: Uses OpenAI's GPT-4o for high-quality itineraries (requires OpenAI API key)
-**- local_travel_agent.py**: Uses Ollama for local LLM inference without sending data to external APIs (requires Ollama to be installed and running)
+**### travel_agent.py**: Uses OpenAI's GPT-4o for high-quality itineraries (requires OpenAI API key)

+**### local_travel_agent.py**: Uses Ollama for local LLM inference without sending data to external APIs

+#### ⚠️ Pré-requisitos para Local Mode

+O local travel agent requer [Ollama](https://ollama.com/) instalado e rodando:

+```bash
+# Linux/macOS
+curl -fsSL https://ollama.com/install.sh | sh
+ollama serve  # or: systemctl start ollama

+# Windows
+# Download from https://ollama.com/download
+# Start via Services or: ollama.exe serve

+# Pull required models
+ollama pull llama3.2
+```

+#### 🔧 Executando em Local Mode

+```bash
+streamlit run local_travel_agent.py
+```

+Enter your SerpAPI key when prompted. The agent will use the locally-running Ollama Llama 3.2 model for planning.

### 📚 Resources

-[Step-by-step tutorials on Unwind AI](https://www.theunwindai.com)
-**[Quick start](#-run-one-now)**
-**[Browse all templates](#-browse-all-templates)**
+[Step-by-step tutorials on Unwind AI](https://www.theunwindai.com) · **[Quick start](#-run-one-now)** · **[Browse all templates](#-browse-all-templates)**
```

**Motivo:** Bug MÉDIA - Usuário tenta rodar o local travel agent sem Ollama e falha porque não há documentação clara sobre os pré-requisitos.

**Benefício:** Reduz frustração do usuário e melhora a experiência de desenvolvimento.

---

## Outros PRs SUGERIDOS (menos prioritários)

### PR #4: Documentar Skills CLI
- **Arquivo:** `README.md`
- **Bug:** Equivocante mencionar `npx skills add` sem documentar instalação
- **Nível:** BAIXA

### PR #5: Corrigir .env.example em multiple locais
- **Arquivo:** `backend/.env.example` em vários agentes
- **Bug:** `EXA_API_KEY=EXA_API_KEY` deveria ser `EXA_API_KEY=your_api_key_here`
- **Nível:** BAIXA

### PR #6: Regenerar registry.json
- **Arquivo:** `agent_skills/registry.json`
- **Bug:** `self-improving-agent-skills` não está no registry
- **Nível:** MÉDIA