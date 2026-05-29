# Week 1 — Setting Up n8n Locally Using Docker

## Task Description
Install and run n8n locally on your laptop using Docker Desktop, confirm the service starts successfully, and access the n8n web interface through the browser.

## Setup Method
Docker Desktop

---

## Step-by-Step Setup

### Step 1 — Verify Docker Installation
```bash
docker -v
# Docker version 27.5.1, build 9f9e405
```

### Step 2 — Pull the n8n Docker Image
```bash
docker pull docker.n8n.io/n8nio/n8n:latest
```

### Step 3 — Create a Docker Volume
```bash
docker volume create n8n_data
```

### Step 4 — Run the n8n Container
```bash
docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n:latest
```

### Step 5 — Confirm the Container is Running
```bash
docker ps
```
Output showed the n8n container with status `Up` and port `0.0.0.0:5678->5678/tcp`.

### Step 6 — Verify Image in Docker Desktop
Opened Docker Desktop and confirmed the `docker.n8n.io/n8nio/n8n` image (2.26 GB) was listed under the Images tab with tag `latest`.

### Step 7 — Access n8n in the Browser
Navigated to:
```
http://localhost:5678
```

### Step 8 — Create Owner Account
Completed the owner account registration form with a name and email, then logged in successfully.

### Step 9 — Access the n8n Dashboard
After login, the n8n workflow dashboard loaded successfully — confirming n8n is fully operational.

---

## Outcome
n8n was successfully installed and is running locally via Docker Desktop. The web interface is accessible at `http://localhost:5678` and is ready for workflow automation.

## Screenshots
See `/screenshots` folder.
