# Week 3 — Claude Desktop + HexStrike MCP + Passive Subdomain Recon

## Task Description
Connect Claude Desktop to the Hexstrike MCP server and confirm that Claude can access the Hexstrike tools. Use Claude to run passive subdomain enumeration against the CSN domain `cloudsecnetwork.com`. Collect subdomains from public sources, review the results, and identify notable findings such as login pages, cloud assets, repeated naming patterns, or unusual subdomains.

## Deliverable
- Screenshot of Claude Desktop connected to the Hexstrike MCP server
- Screenshot of a subdomain enumeration query against the CSN domain
- Claude's summary report

---

## Step-by-Step Setup

### Prerequisites
- Windows 10/11
- Claude Desktop installed (Windows Store version)
- Git Bash terminal

### Step 1 — Verify Prerequisites
Open Git Bash and confirm Python and Git are installed:
```bash
python --version   # Python 3.13.2
git --version      # git version 2.47.1.windows.2
```

### Step 2 — Create Project Folder
```bash
mkdir /c/hexstrike-ai
cd /c/hexstrike-ai
```

### Step 3 — Clone HexStrike Repository
```bash
git clone https://github.com/0x4m4/hexstrike-ai.git .
```

### Step 4 — Create Python Virtual Environment
```bash
python -m venv hexstrike-env
source hexstrike-env/Scripts/activate
```
> **Note:** On Windows with Git Bash, use `source hexstrike-env/Scripts/activate` (forward slashes).
> `hexstrike-env\Scripts\activate` will fail with "command not found".

### Step 5 — Install Dependencies
```bash
export PYTHONUTF8=1
pip install -r requirements.txt
```
> **Note:** `export PYTHONUTF8=1` is required on Windows to fix a `UnicodeDecodeError: 'charmap' codec can't decode byte` error that occurs without it.

### Step 6 — Start HexStrike Server
```bash
python hexstrike_server.py
```
Expected output:
```
[INFO] Server starting on 127.0.0.1:8888
[INFO] 150+ integrated modules | Adaptive AI decision engine active
* Running on http://127.0.0.1:8888
```
> **Keep this terminal open.** The server must stay running while using Claude Desktop.

### Step 7 — Configure Claude Desktop MCP

#### Finding the correct config file location
Claude Desktop on Windows (installed via Windows Store) uses a **virtualized filesystem**. The config file is NOT at the standard path. The correct location is:

```
C:\Users\<your-username>\AppData\Local\Packages\Claude_pzs8sxrjxfjjc\LocalCache\Roaming\Claude\claude_desktop_config.json
```

> **Tip:** The easiest way to find and open the correct config file is:
> Claude Desktop → Settings → Developer → **Edit Config**

#### Config file content
```json
{
  "mcpServers": {
    "hexstrike-ai": {
      "command": "C:/hexstrike-ai/hexstrike-env/Scripts/python.exe",
      "args": [
        "C:/hexstrike-ai/hexstrike_mcp.py",
        "--server",
        "http://127.0.0.1:8888"
      ],
      "timeout": 300,
      "disabled": false
    }
  }
}
```

### Step 8 — Restart Claude Desktop
Fully quit Claude Desktop and reopen it. To verify the connection:
- Go to **Settings → Developer**
- You should see `hexstrike-ai` with a green **"running"** badge

### Step 9 — Enable HexStrike in Chat
- Click the **`+`** button in the chat input
- Click **Connectors**
- Toggle **hexstrike-ai** on (blue)

---

## Running Passive Subdomain Enumeration

In Claude Desktop, type:
```
Run passive subdomain enumeration on cloudsecnetwork.com and give me a summary report of findings
```

Claude will use HexStrike tools (Subfinder, Amass) and fall back to DNS resolution, Google OSINT, and certificate transparency logs.

---

## Results — cloudsecnetwork.com

**Date:** 2026-05-28
**Methods:** DNS resolution, Google search index OSINT, reverse DNS lookups

### Confirmed Live Subdomains

| Subdomain | Resolved IPs | Infrastructure | Source |
|-----------|-------------|----------------|--------|
| cloudsecnetwork.com | 18.238.176.x | CloudFront (den53) | DNS |
| www.cloudsecnetwork.com | 18.238.176.x | CloudFront (den53) | DNS |
| learn.cloudsecnetwork.com | 52.45.101.119, 32.197.142.236 | EC2 us-east-1 | DNS, Google |
| api.cloudsecnetwork.com | 52.45.101.119, 32.197.142.236 | EC2 us-east-1 | DNS |
| docs.cloudsecnetwork.com | 99.84.118.x | CloudFront (den53) | DNS |
| cdn.cloudsecnetwork.com | 18.238.136.x | CloudFront (den53) | DNS |
| blog.cloudsecnetwork.com | — | Not resolved via DNS | Google |

### Key Findings

**🔴 EC2 origin IPs exposed on learn.* and api.***
Unlike the main site (behind CloudFront), the LMS and API endpoints expose raw EC2 IPs directly. If security groups allow 0.0.0.0/0 on ports 80/443, the origin can be attacked directly, bypassing any WAF at the CloudFront layer.

**🟠 Multiple CloudFront distributions with no centralized WAF**
Three separate CloudFront distributions identified (apex/www, docs, cdn). Without an AWS WAF WebACL attached to each, they are independently unprotected.

**🟡 LMS platform (learn.*) runs on same cluster as api.***
learn.cloudsecnetwork.com and api.cloudsecnetwork.com share identical IPs, suggesting tight coupling — a compromise of one could affect the other.

**🟢 No dangling CNAMEs detected**
All live subdomains resolve to valid IP addresses — no subdomain takeover risk.

**🟢 Small, well-defined attack surface**
No dev, staging, admin, vpn, or internal subdomains found publicly exposed.

---

## Troubleshooting

| Issue | Cause | Fix |
|-------|-------|-----|
| `hexstrike-env\Scripts\activate` fails | Git Bash uses Unix-style paths | Use `source hexstrike-env/Scripts/activate` |
| `UnicodeDecodeError` during pip install | Windows encoding issue | Run `export PYTHONUTF8=1` before pip install |
| Config file not found at `AppData\Roaming\Claude` | Windows Store app uses virtualized filesystem | Use Settings → Developer → Edit Config to find correct path |
| Hammer icon not showing in Claude Desktop | Config not loaded yet | Fully quit and restart Claude Desktop after saving config |

---

## Screenshots

| File | Description |
|------|-------------|
| `HexStrike_run.png` | HexStrike server running on port 8888 |
| `1.png` | Claude Desktop running subdomain enumeration query |
| `2.png` | Confirmed subdomains with IPs and infrastructure analysis |
| `3.png` | Reconnaissance findings |
| `4.png` | Claude's summary report |
