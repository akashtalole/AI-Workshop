---
title: "RECRUIT-02: Setting Up Your AI Dev Environment"
date: 2026-04-24 10:00:00 +0530
categories: [Recruit, Foundations]
tags: [environment, python, virtual-env, vscode, anthropic-sdk, recruit]
description: "Set up a professional, reproducible AI development environment with Python virtual environments, the Anthropic SDK, and useful tooling."
toc: true
author: akashtalole
---

## Mission Brief

A solid development environment saves hours of debugging later. This mission walks you through setting up a professional Python environment specifically configured for AI development.

> **Track:** Recruit `•` | **Time:** 25 minutes | **Prerequisites:** [RECRUIT-01](/posts/recruit-01-intro-llms/)

## Learning Objectives

By the end of this mission, you will:

1. Create isolated Python virtual environments for AI projects
2. Structure a project with proper dependency management
3. Safely manage API keys using `.env` files
4. Set up VS Code with AI development extensions
5. Write and run your first structured AI project

## Project Structure

A well-organized AI project looks like this:

```
my-ai-project/
├── .env               # API keys (NEVER commit this)
├── .env.example       # Template showing required env vars
├── .gitignore         # Excludes .env, __pycache__, etc.
├── requirements.txt   # Python dependencies
├── README.md
└── src/
    └── main.py        # Your application code
```

## Hands-On Lab

### Step 1: Create the Project

```bash
# Create and enter your project directory
mkdir my-ai-project && cd my-ai-project

# Create a Python virtual environment
python3 -m venv .venv

# Activate it
# macOS/Linux:
source .venv/bin/activate
# Windows:
# .venv\Scripts\activate

# Your prompt should now show (.venv)
```

### Step 2: Install Dependencies

```bash
# Install the Anthropic SDK and dotenv for env management
pip install anthropic python-dotenv

# Save your dependencies
pip freeze > requirements.txt
```

### Step 3: Secure API Key Management

Create `.env`:

```bash
# .env — DO NOT COMMIT THIS FILE
ANTHROPIC_API_KEY=sk-ant-your-key-here
```

Create `.env.example` (safe to commit — shows what's needed without the values):

```bash
# .env.example
ANTHROPIC_API_KEY=your-api-key-here
```

Create `.gitignore`:

```gitignore
.env
.venv/
__pycache__/
*.pyc
.DS_Store
```

### Step 4: Write Your First Structured Application

Create `src/main.py`:

```python
import os
from dotenv import load_dotenv
import anthropic

load_dotenv()

def create_client() -> anthropic.Anthropic:
    api_key = os.getenv("ANTHROPIC_API_KEY")
    if not api_key:
        raise ValueError("ANTHROPIC_API_KEY not set in .env file")
    return anthropic.Anthropic(api_key=api_key)

def ask(client: anthropic.Anthropic, question: str, system: str = "") -> str:
    kwargs = {
        "model": "claude-sonnet-4-6",
        "max_tokens": 512,
        "messages": [{"role": "user", "content": question}],
    }
    if system:
        kwargs["system"] = system

    message = client.messages.create(**kwargs)
    return message.content[0].text

def main():
    client = create_client()

    system = "You are a concise AI tutor. Give clear, short answers."
    question = "What is the difference between supervised and unsupervised learning?"

    print("Question:", question)
    print("\nAnswer:", ask(client, question, system))

if __name__ == "__main__":
    main()
```

Run it:

```bash
python src/main.py
```

### Step 5: VS Code Setup (Optional but Recommended)

Install these extensions:
- **Python** (`ms-python.python`) — Language support
- **Pylance** (`ms-python.vscode-pylance`) — Type checking
- **GitHub Copilot** — AI code completion (free for students)
- **REST Client** — Test API calls from `.http` files

Create `.vscode/settings.json`:

```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
  "editor.formatOnSave": true,
  "[python]": {
    "editor.defaultFormatter": "ms-python.python"
  }
}
```

---

## Mission Complete

Your AI dev environment is production-ready:

- [x] Python virtual environment isolated from system Python
- [x] Dependencies pinned in `requirements.txt`
- [x] API keys secured in `.env`, never committed
- [x] Structured, reusable project layout
- [x] VS Code configured for Python AI development

---

## Navigation

**← Previous:** [RECRUIT-01: Introduction to AI & LLMs](/posts/recruit-01-intro-llms/)  
**Next Mission →** [RECRUIT-03: Your First AI Application](/posts/recruit-03-first-app/)
