# AI and Security — Weekly Tasks

This repository documents weekly cybersecurity tasks covering n8n workflow automation, AI-powered reconnaissance, and penetration testing using Claude Desktop, HexStrike MCP, and CAI framework.

## Repository Structure

```
AI-and-Security/
├── week-1/          # n8n local setup using Docker
├── week-2/          # Phishing triage workflow + Google Sheets logging
├── week-3/          # Claude Desktop + HexStrike MCP + subdomain recon
├── week-4/          # CAI framework + authorized web app penetration test
└── workflows/       # Exported n8n workflow JSON files
```

## Week Overview

| Week | Task | Tools | Status |
|------|------|-------|--------|
| Week 1 | Set up n8n locally using Docker Desktop | Docker, n8n | ✅ Done |
| Week 2 | Import phishing triage workflow + add Google Sheets logging | n8n, Google Sheets, ngrok, Gemini AI | ✅ Done |
| Week 3 | Connect Claude Desktop to HexStrike MCP and run passive subdomain enumeration on cloudsecnetwork.com | Claude Desktop, HexStrike AI, Python | ✅ Done |
| Week 4 | Set up CAI framework and run authorized web app penetration test on CSN lab | CAI, Ollama, llama3, WSL2, Ubuntu | ✅ Done |

## Key Findings Summary

### Week 3 — Subdomain Recon (cloudsecnetwork.com)
- 6 live subdomains discovered
- `learn.*` and `api.*` expose raw EC2 IPs — no CloudFront WAF protection
- No dangling CNAMEs detected

### Week 4 — Web App Pentest (vaultchat-csn.fly.dev)
- Version disclosure: `VaultChat/1.2.0` exposed in HTTP headers
- AI tech stack exposed: `openai/gpt-3.5-turbo` visible in response headers
- Server infrastructure visible: `Fly/410b5f3c1`

## Tools Used

| Tool | Purpose |
|------|---------|
| [n8n](https://n8n.io) | Workflow automation platform |
| [Docker Desktop](https://www.docker.com/products/docker-desktop/) | Container runtime for n8n |
| [ngrok](https://ngrok.com) | Webhook tunneling for local n8n |
| [Claude Desktop](https://claude.ai/download) | AI assistant with MCP support |
| [HexStrike AI](https://github.com/0x4m4/hexstrike-ai) | AI-powered cybersecurity MCP server |
| [CAI Framework](https://github.com/aliasrobotics/cai) | Cybersecurity AI penetration testing framework |
| [Ollama](https://ollama.com) | Local AI model runner (llama3) |
| Google Sheets | Incident logging for phishing triage |
