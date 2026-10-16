# Bugs Encontrados

## Bug #1: Inconsistência de Gerenciador de Dependências

**Localização:** `advanced_ai_agents/multi_agent_apps/agent_teams/ai_travel_planner_agent_team/README.md`

**Descrição:** O README menciona instalação via `pip install -r requirements.txt`, mas o projeto utiliza `pyproject.toml` e `uv.lock` como gerenciador de dependências (uv).

**Passos para reproduzir:**
1. Navegar para `advanced_ai_agents/multi_agent_apps/agent_teams/ai_travel_planner_agent_team/`
2. Verificar que não existe `requirements.txt`
3. Verificar que existe `pyproject.toml` e `uv.lock`
4. Seguir instruções do README: `pip install -r requirements.txt`
5. Resultado: `FileNotFoundError: requirements.txt not found`

**Severidade:** CRÍTICA

**Causa raiz:** O README foi mantido com instruções antigas de `pip install -r requirements.txt` após a migração para uv como gerenciador de dependências. O arquivo `uv.lock` está presente, indicando uso intencional do uv.

**Correção mínima proposta:**

```markdown
### Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Linux/Mac/Windows: venv\Scripts\activate

# Install dependencies (using uv package manager)
pip install uv  # if not already installed
uv pip install pyproject.toml

# Or use pip directly (deprecated but works)
pip install -r <(python -c "import tomllib; d=tomllib.load(open('pyproject.toml','rb')); print('\\n'.join(d['project']['dependencies']))")
```

Alternativamente, manter requirements.txt como arquivo gerado:
```bash
python -c "import tomllib; d=tomllib.load(open('pyproject.toml','rb')); [print(f'{p}') for p in d['project']['dependencies']]" > requirements.txt
```
```

---

## Bug #2: Arquivo .env.example Faltando

**Localização:** `agent_skills/self-improving-agent-skills/backend/`

**Descrição:** O README instrui o usuário a executar `cp .env.example .env`, mas o arquivo `.env.example` não existe no diretório backend.

**Passos para reproduzir:**
1. Navegar para `agent_skills/self-improving-agent-skills/backend/`
2. Tentar executar: `cp .env.example .env`
3. Resultado: `cp: cannot stat '.env.example': No such file or directory`

**Severidade:** MÉDIA

**Causa raiz:** O arquivo de template de variáveis de ambiente foi omitido durante o desenvolvimento. O backend requer `GOOGLE_API_KEY` e opcionalmente `GEMINI_MODEL` para funcionar.

**Correção mínima proposta:**

Criar o arquivo `.env.example` com o seguinte conteúdo:

```bash
# Google AI Studio API Key - get it from https://aistudio.google.com/apikey
GOOGLE_API_KEY=your_google_api_key_here

# Optional: Override the default Gemini model
# Default: gemini-3.8-flash
GEMINI_MODEL=gemini-3.8-flash
```

---

## Bug #3: Timeout de Instalação no AI Travel Agent

**Localização:** `starter_ai_agents/ai_travel_agent/`

**Descrição:** A instalação das dependências via `pip install -r requirements.txt` expira devido ao tamanho do pacote `agno` e suas dependências transitivas.

**Passos para reproduzir:**
1. `cd starter_ai_agents/ai_travel_agent`
2. `pip install -r requirements.txt`
3. Após 120 segundos: timeout

**Severidade:** MÉDIA

**Causa raiz:** O pacote `agno` possui muitas dependências (OpenAI, Anthropic, Google, etc.) e o timeout padrão do terminal é insuficiente.

**Correção mínima proposta:**

Adicionar ao README:
```bash
# Increase timeout for large packages
pip install --timeout 300 --retries 3 -r requirements.txt

# Or use uv for faster installation
pip install uv
uv pip install -r requirements.txt
```

---

## Bug #4: Ollama Não Documentado como Pré-requisito

**Localização:** `starter_ai_agents/ai_travel_agent/README.md`

**Descrição:** O local travel agent usea Ollama como modelo LLM, mas o README não documenta que o Ollama deve estar instalado e rodando.

**Passos para reproduzir:**
1. Instalar dependências
2. Tentar rodar `streamlit run local_travel_agent.py`
3. Erro: `Ollama not available or not running`

**Severidade:** MÉDIA

**Causa raiz:** A documentação menciona "For local LLM usage (with Ollama)" mas não detalha que:
1. Ollama deve estar instalado
2. Ollama deve estar rodando como serviço
3. Os modelos devem ser pullados previamente

**Correção mínima proposta:**

Adicionar seção no README:
```markdown
### Pré-requisitos para Local Travel Agent

O local travel agent requer Ollama instalado e rodando:

```bash
# Linux/macOS
curl -fsSL https://ollama.com/install.sh | sh
ollama serve  # ou: systemctl start ollama

# Windows
# Download from https://ollama.com/download
# Start via Services or: ollama.exe serve

# Pull required models
ollama pull llama3.2
```
```

---

## Bug #5: Skills CLI não Documentado

**Localização:** `README.md` (linha 83)

**Descrição:** O README mostra o comando `npx skills add <url>` mas não documenta como instalar o Skills CLI.

**Passos para reproduzir:**
1. Novo usuário lê README
2. Tentativa: `npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/project-graveyard`
3. Resultado: `npx: command not found: skills`

**Severidade:** BAIXA

**Causa raiz:** O Skills CLI é uma ferramenta externa não documentada no README.

**Correção mínima proposta:**

Adicionar seção no README:
```markdown
### Instalação do Skills CLI

Se você não tem o Skills CLI instalado:

```bash
npm install -g @skills.sh/cli
# or
corepack enable && corepack prepare @skills.sh/cli --activate
```

Verifique a instalação:
```bash
npx skills --version
```
```

---

## Bug #6: registry.json não inclui self-improving-agent-skills

**Localização:** `agent_skills/registry.json`

**Descrição:** A skill `self-improving-agent-skills` está listada no README.md (`agent_skills/` section) mas não aparece no `registry.json`.

**Passos para reproduzir:**
1. Verificar `agent_skills/README.md` - skill está listada
2. Verificar `agent_skills/registry.json` - skill não aparece
3. Executar: `python3 agent_skills/evals/tools/test_registry_lint.py`
4. Resultado: Erro sobre skill faltando do registry

**Severidade:** MÉDIA

**Causa raiz:** O registry foi gerado sem a skill self-improving-agent-skills, ou a skill não passou pelo processo de registro.

**Correção mínima proposta:**

Executar:
```bash
python3 agent_skills/evals/tools/registry_lint.py --write
```

Ou verificarificar se a skill tem SKILL.md válido e adicioná-la ao registry manualmente.

---

## Bug #7: PATH de instalação do skill graveyard inconsistente

**Localização:** `agent_skills/project-graveyard/README.md` (linha 50)

**Descrição:** O README mostra como instalar via skills CLI e como rodar standalone. No modo standalone, o caminho é:
```bash
python3 project-graveyard/scripts/graveyard.py ~/dev ~/projects
```

Mas a estrutura real mostra:
```
agent_skills/project-graveyard/scripts/graveyard.py
```

**Passos para reproduzir:**
1. Clonar o repo
2. Executar: `python3 agent_skills/project-graveyard/scripts/graveyard.py`
3. Funciona corretamente

**Severidade:** BAIXA (apenas confusão na documentação)

**Causa raiz:** O README mostra o caminho como se o projeto já estivesse clonado em um diretório `project-graveyard/`, mas na verdade o usuário clona o repo completo e navega até `agent_skills/project-graveyard/`.

**Correção mínima proposta:**

```bash
# Do root do repo
python3 agent_skills/project-graveyard/scripts/graveyard.py ~/dev ~/projects

# Ou instale via skills CLI e rode de qualquer lugar
npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/project-graveyard
python3 scripts/graveyard.py ~/dev ~/projects
```

---

## Bug #8: Comando errado no .env.example

**Localização:** `agent_skills/project-graveyard/backend/.env.example` (se existisse)

**Descrição:** O campo `EXA_API_KEY` tem valor incorreto:
```
EXA_API_KEY=EXA_API_KEY
```

Deveria ser:
```
EXA_API_KEY=your_exa_api_key_here
```

**Severidade:** BAIXA

**Causa raiz:** Erro de digitação ou placeholder não substituído.

**Correção mínima proposta:**

Corrigir para:
```
EXA_API_KEY=your_exa_api_key_here
```

O bug pode ser corrigido editando o arquivo diretamente ou, se não existir, criando um novo com os valores corretos.