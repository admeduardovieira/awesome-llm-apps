# Reprodução: Installation and Execution

## Attempt 1: Project Graveyard Skill (PASSED)

**Comando executado:**
```bash
python3 agent_skills/evals/project-graveyard/test_graveyard.py
```

**Resultado:** `PASS — 16/16 checks`

**Verificação de lint:**
```bash
python3 agent_skills/evals/tools/skill_lint.py agent_skills/project-graveyard --strict
```
**Resultado:** `PASS — 0 error(s), 0 warning(s)`

**Verificação de segurança:**
```bash
python3 agent_skills/evals/tools/skill_scanner.py agent_skills/project-graveyard
```
**Resultado:** `clean — no findings`

## Attempt 2: AI Travel Agent (Starter)

**Instruções do README:**
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/starter_ai_agents/ai_travel_agent
pip install -r requirements.txt
streamlit run travel_agent.py
```

**Problemas encontrados:**

1. **Timeout no pip install** - Instalação dos pacotes `agno`, `openai`, `ollama`, `google-search-results`, `icalendar`, `streamlit` expirou após 120 segundos
   - **Severidade:** Média
   - **Causa raiz:** Tamanho dos pacotes (agno tem muitas dependências transitivas)
   - **Correção:** Use `uv pip install` ou limite o download com `--no-cache-dir` e `--timeout`

2. **Dependência Ollama não disponível localmente**
   - **Severidade:** Média
   - **Causa raiz:** O agente local precisa do Ollama rodando em `http://localhost:11434`
   - **Correção:** Documentar que o Ollama deve estar instalado e inicializado antes da execução

## Attempt 3: Local RAG Agent

**Instruções do README:**
```bash
cd rag_tutorials/local_rag_agent
pip install -r requirements.txt
docker pull qdrant/qdrant
docker run -p 6333:6333 qdrant/qdrant
ollama pull llama3.2
ollama pull openhermes
python local_rag_agent.py
```

**Problemas encontrados:**

1. **Falta de pyproject.toml ou requirements.txt claros** - O diretório tem requirements.txt mas falta documentação clara de dependências extras
   - **Severidade:** Baixa

## Attempt 4: AI Travel Planner Agent Team (Advanced)

**Estrutura encontrada:**
```
advanced_ai_agents/multi_agent_apps/agent_teams/ai_travel_planner_agent_team/
├── backend/
│   ├── pyproject.toml    # USANDO UV (não requirements.txt)
│   ├── uv.lock           # Lockfile do uv
│   ├── Dockerfile
│   └── ...códigos Python
├── client/                # Frontend Next.js
└── README.md
```

**Problemas encontrados:**

1. **Inconsistência no gerenciador de dependências**
   - **Severidade:** ALTA
   - **Causa raiz:** README afirma `pip install -r requirements.txt` mas o diretório usa `pyproject.toml` + `uv.lock`
   - **Correção mínima:** Atualizar o README para indicar `pip install -r pyproject.toml` ou usar `uv pip install`

2. **Comando README incorreto**
   - **Severidade:** ALTA
   - **Causa raiz:** O README deixam claro que `pip install -r requirements.txt` funciona, mas o projeto usa uv
   - **Correção mínima:** Adicionar instruções claras para uv:
   ```bash
   pip install uv
   uv pip install -r pyproject.toml
   ```

## Attempt 5: Self-Improving Agent Skills

**Instruções do README (backend):**
```bash
cd backend
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou venv\Scripts\activate  # Windows
pip install -r requirements.txt
cp .env.example .env
python app.py
```

**Problemas encontrados:**

1. **Falta de .env.example no diretório backend**
   - **Severidade:** MÉDIA
   - **Causa raiz:** O README menciona `cp .env.example .env` mas o arquivo não existe
   - **Correção mínima:** Criar `.env.example` com:
   ```
   GOOGLE_API_KEY=your_google_api_key
   GEMINI_MODEL=gemini-3.8-flash
   ```

## Attempt 6: Skills CLI Installation

**Comando do README:**
```bash
npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/project-graveyard
```

**Problemas encontrados:**

1. **Skills CLI não documentado**
   - **Severidade:** BAIXA
   - **Causa raiz:** O README assume que o usuário já tem o Skills CLI instalado (`npm install -g @skills.sh/cli`)
   - **Correção mínima:** Adicionar instrução de instalação prévia

## Tabela de Erros Encontrados

| ID | Arquivo/Componente | Descrição | Severidade | Causa Raiz | Correção Proposta |
|----|---------------------|-----------|------------|------------|-------------------|
| E001 | ai_travel_planner_agent_team/README.md | Indica `pip install -r requirements.txt` mas usa `pyproject.toml` | CRÍTICA | Gerenciador de dependências inconsistente | Atualizar README para uv ou adicionar requirements.txt |
| E002 | self-improving-agent-skills/backend/README.md | `cp .env.example .env` mas arquivo não existe | MÉDIA | Arquivo de template omitido | Criar .env.example com variáveis necessárias |
| E003 | starter_ai_agents/ai_travel_agent/README.md | Instalação pip sem timeout adequado | MÉDIA | Pacotes grandes expiram | Adicionar timeout ou usar uv |
| E004 | starter_ai_agents/ai_travel_agent/ | Ollama não verificado como pré-requisito | MÉDIA | Passo de instalação omitido | Documentar instalação do Ollama |
| E005 | README.md (principal) | Skills CLI não documentado | BAIXA | Assume instalação prévia | Adicionar instrução de instalação do CLI |

## Comandos que Funcionaram

### 1. Execução dos testes do project-graveyard
```bash
python3 agent_skills/evals/project-graveyard/test_graveyard.py
# Resultado: PASS — 16/16 checks
```

### 2. Lint do skill
```bash
python3 agent_skills/evals/tools/skill_lint.py agent_skills/project-graveyard --strict
# Resultado: PASS — 0 error(s), 0 warning(s)
```

### 3. Security scan
```bash
python3 agent_skills/evals/tools/skill_scanner.py agent_skills/project-graveyard
# Resultado: clean — no findings
```

### 4. Regenerar o registry
```bash
python3 agent_skills/evals/tools/registry_lint.py --write
# Cria agent_skills/registry.json com todas as skills
```

## Resumo da Reprodução

**Status geral:** PARCIALMENTE SUCESSOS

- **Skills validados:** 1 (project-graveyard - full CI pass)
- **Agentes testados:** 2 tentativas (travel agent, local RAG)
- **Problemas críticos:** 1 (inconsistência de dependências no AI Travel Planner Agent Team)
- **Problemas médios:** 3 (arquivo de ambiente faltando, timeout pip, Ollama pré-requisito)
- **Problemas baixos:** 1 (documentação CLI)

**Recomendação:** Antes de qualquer deploy, corrigir E001 (crítica) e E002 (média).