# Week 4 — CAI Framework + Authorized Web Application Penetration Test

## Task Description
Install and configure the Cybersecurity AI (CAI) framework on a local system, define the approved target, scope, and rules of engagement, then use CAI to perform an authorized penetration test against the CSN learning platform and document key findings.

## Environment
- **OS:** Ubuntu 24.04.2 LTS (WSL2 on Windows)
- **AI Backend:** Ollama llama3 (local, no cloud API key required)
- **CAI Version:** 0.5.10
- **Working Directory:** `~/cai-work`

---

## Step-by-Step Setup

### Step 1 — Set Up Ubuntu Environment (WSL2)
Launched Ubuntu 24.04.2 LTS via WSL2 and updated the system:
```bash
sudo apt-get update && sudo apt-get install -y python3-pip python3.12-venv
```

### Step 2 — Create Virtual Environment and Install CAI
```bash
python3.12 -m venv ~/cai-env && source ~/cai-env/bin/activate
pip install cai-framework
```
CAI v0.5.10 installed along with dependencies: litellm, mcp, flask, numpy, and others.

### Step 3 — Install Ollama and Pull llama3 Model
```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama pull llama3
```
- llama3 model: 4.7 GB downloaded successfully
- Ollama API started at `http://127.0.0.1:11434`

### Step 4 — Configure CAI Environment Variables
Create a `.env` file in your working directory:
```bash
cat > .env <<'EOF'
CAI_MODEL="ollama/llama3"
OLLAMA_API_BASE="http://localhost:11434"
CAI_AGENT_TYPE="one_tool_agent"
CAI_STREAM=true
CAI_WORKSPACE="default"
