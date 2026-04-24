---
layout: page
title: User Guide
icon: fas fa-book-reader
order: 5
---

# AI-Workshop User Guide

Welcome to AI-Workshop. This guide explains everything you need to know to get the most out of the platform.

---

## Table of Contents

1. [Platform Overview](#1-platform-overview)
2. [Getting Started](#2-getting-started)
3. [Mission Tracks](#3-mission-tracks)
4. [Reading a Mission](#4-reading-a-mission)
5. [Hands-On Labs](#5-hands-on-labs)
6. [Comments & Community](#6-comments--community)
7. [Search & Navigation](#7-search--navigation)
8. [Progress Tracking](#8-progress-tracking)
9. [Troubleshooting](#9-troubleshooting)
10. [Contributing](#10-contributing)

---

## 1. Platform Overview

AI-Workshop is a **mission-based AI education platform**. Instead of passive reading, every topic is delivered as a structured mission with:

- A clear goal and time estimate
- Conceptual background (brief — just enough to act)
- A hands-on lab you run on your own machine
- A completion checklist

The platform is modeled after the way special operations teams train: progressive difficulty, clear objectives, real-world application.

### Who Is This For?

| Level | Background | Start Here |
|-------|-----------|-----------|
| Complete beginner to AI | Software developer new to AI | [RECRUIT-00](/posts/recruit-00-orientation/) |
| Some AI exposure | Know Python, have tried ChatGPT | [RECRUIT-01](/posts/recruit-01-intro-llms/) |
| Intermediate | Have used the OpenAI or Anthropic API | [OPERATIVE-01](/posts/operative-01-ai-agents/) |
| Specific skill gap | Any experience level | [Special Ops](/categories/special-ops/) |

---

## 2. Getting Started

### Step 1: Accounts You'll Need

Before beginning any lab, create these accounts:

| Account | Purpose | URL |
|---------|---------|-----|
| **GitHub** | Code hosting, platform comments, issue tracking | [github.com](https://github.com) |
| **Anthropic Console** | Claude API key for all labs | [console.anthropic.com](https://console.anthropic.com) |

### Step 2: Get a Claude API Key

1. Sign in at [console.anthropic.com](https://console.anthropic.com)
2. Left sidebar → **API Keys** → **Create Key**
3. Name it `ai-workshop` (for your own reference)
4. Copy the key immediately — it won't be shown again
5. Store it in a password manager

> **Free tier:** Anthropic offers a free tier with limited tokens. It's sufficient for all Recruit-track missions. Operative missions may require a small credit top-up ($5–10 covers extensive practice).

### Step 3: Set Up Python

All lab code uses Python 3.10+:

```bash
# Check your version
python3 --version   # should be 3.10 or higher

# Install the Anthropic SDK
pip install anthropic python-dotenv
```

### Step 4: Configure Your API Key

Create a `.env` file in your project directory:

```
ANTHROPIC_API_KEY=sk-ant-your-key-here
```

Load it in your scripts:

```python
from dotenv import load_dotenv
import os

load_dotenv()
api_key = os.getenv("ANTHROPIC_API_KEY")
```

> **Never commit `.env` to Git.** Add `.env` to your `.gitignore`.

---

## 3. Mission Tracks

### Track Progression

```
Recruit (•)  →  Operative (••)  →  Commander (•••)
                                        ↑
          Special Ops ─────────── any time ──┘
```

Tracks build on each other. **Recruit must be completed before Operative.** Special Ops can be taken any time — they have no prerequisites.

### Recruit Track `•`

**5 missions | ~2.5 hours total**

The foundational track. You'll go from zero to building a working multi-turn AI chat application with proper prompt engineering.

| Mission | What You Build |
|---------|---------------|
| RECRUIT-00 | Platform orientation, API key setup |
| RECRUIT-01 | Mental model of LLMs, temperature experiments |
| RECRUIT-02 | Professional dev environment with `.env` management |
| RECRUIT-03 | Multi-turn streaming chat application |
| RECRUIT-04 | Few-shot classifiers, CoT reasoner, structured output extractor |

### Operative Track `••`

**5 missions | ~6.5 hours total**

Intermediate track. You'll build agents, connect them to tools and data, orchestrate multi-agent pipelines, and harden them for safety.

| Mission | What You Build |
|---------|---------------|
| OPERATIVE-01 | Goal-directed agent loop, memory-aware assistant |
| OPERATIVE-02 | Multi-tool agent with real function dispatch |
| OPERATIVE-03 | Full RAG pipeline with vector search |
| OPERATIVE-04 | Parallel agent execution, orchestrator-worker system |
| OPERATIVE-05 | Injection defense, red-team evaluation suite |

### Commander Track `•••`

Advanced production systems track — **coming soon**. Topics will include latency budgets, fallback chains, observability, and enterprise deployment patterns.

[Watch the repo](https://github.com/akashtalole/AI-Workshop) to be notified when Commander missions launch.

### Special Ops

Standalone missions on specific tools — take them whenever they're relevant to you.

| Mission | What You Learn |
|---------|---------------|
| SPECIAL-OPS-01 | Build a custom MCP server, connect to Claude Desktop |
| SPECIAL-OPS-02 | Claude Code CLI: CLAUDE.md, slash commands, permissions |
| SPECIAL-OPS-03 | Attack and defend AI systems: injection, extraction, red-teaming |

---

## 4. Reading a Mission

Every mission follows this consistent structure:

### Mission Brief

Tells you:
- **Track and difficulty** (e.g., `Operative ••`)
- **Time estimate** (realistic, including lab time)
- **Prerequisites** (which previous missions to complete first)

### Learning Objectives

A numbered list of concrete skills you'll have after the mission. If you can already do all of these confidently, you can safely skip the mission.

### Background / Concepts

Short conceptual explanation — just enough to understand the lab. External links are provided for deeper reading if you want it.

### Hands-On Lab

The core of every mission. Step-by-step instructions with complete, runnable code. Always run the code yourself — reading is not sufficient.

### Mission Complete

A checklist of what you accomplished. Use this to self-assess before moving on.

### Navigation

Links to the previous and next mission. Always follow these to maintain sequence within a track.

---

## 5. Hands-On Labs

### How to Run Lab Code

1. Create a project directory for the track you're on (e.g., `~/ai-workshop/recruit/`)
2. Set up a Python virtual environment:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate   # macOS/Linux
   # .venv\Scripts\activate    # Windows
   ```
3. Install dependencies: `pip install anthropic python-dotenv`
4. Create `.env` with your API key
5. Copy each code block into a `.py` file and run it

### Code Block Conventions

| Style | Meaning |
|-------|---------|
| `python` block | Complete, runnable Python code |
| `bash` block | Terminal commands — run as-is |
| `json` / `yaml` block | Config file content |
| Comments starting `#` | Annotations — not part of the code logic |

### When Code Doesn't Work

Common issues:

**`AuthenticationError`** — Your API key is invalid or not set.
```bash
echo $ANTHROPIC_API_KEY   # Should print your key (not empty)
```

**`RateLimitError`** — You've hit the API rate limit. Wait 60 seconds and retry.

**`ModuleNotFoundError: anthropic`** — SDK not installed in the active environment.
```bash
pip install anthropic   # Make sure your venv is activated
```

**Empty output / no response** — Check `max_tokens` is set to a reasonable value (at least 256 for most labs).

---

## 6. Comments & Community

### Per-Mission Comments (Giscus)

Every mission page has a comment section at the bottom powered by **Giscus** (GitHub Discussions). To leave a comment:

1. Scroll to the bottom of any mission page
2. Sign in with your GitHub account
3. Write your comment — it becomes a GitHub Discussion thread automatically

Use mission comments to:
- Ask questions about specific steps
- Share your code output or screenshots
- Suggest improvements to the mission

### GitHub Discussions

For broader questions, visit the [GitHub Discussions tab](https://github.com/akashtalole/AI-Workshop/discussions):

| Category | Use For |
|----------|---------|
| **Q&A** | "I'm stuck on OPERATIVE-03 — how do I..." |
| **Show and Tell** | "I built X using what I learned in RECRUIT-04" |
| **Ideas** | "Could there be a mission about fine-tuning?" |

### GitHub Issues

Use Issues for actionable reports:

- [**Mission Feedback**](https://github.com/akashtalole/AI-Workshop/issues/new?template=mission-feedback.yml) — Rate a mission, report confusion or missing content
- [**Bug Report**](https://github.com/akashtalole/AI-Workshop/issues/new?template=bug-report.yml) — Broken links, code that doesn't run, rendering issues
- [**Feature Request**](https://github.com/akashtalole/AI-Workshop/issues/new?template=feature-request.yml) — Suggest new missions or tracks

---

## 7. Search & Navigation

### Site Search

Click the 🔍 search icon in the top navigation bar. Search works across all mission titles, descriptions, and content. Useful for finding missions on specific topics (e.g., search "streaming" or "embeddings").

### Sidebar Navigation

| Tab | Contents |
|-----|---------|
| **Home** | Latest missions, pinned orientation post |
| **Categories** | Missions grouped by track (Recruit, Operative, etc.) |
| **Tags** | Cross-cutting topics (e.g., `prompt-engineering`, `rag`, `mcp`) |
| **Archives** | All missions sorted by date |
| **Missions** | Full mission directory table with time estimates |
| **About** | Platform info and contribution guide |

### Categories Page

The most useful navigation for structured learning. Categories are organized as:

```
Recruit
  └── Foundations
  └── Hands-On
  └── Techniques
Operative
  └── Agents
  └── Advanced
  └── Safety
Commander
  └── Architecture
Special-Ops
  └── Tools
  └── Security
```

### Tags

Tags let you jump to missions on a specific topic regardless of track. For example, the `mcp` tag links SPECIAL-OPS-01 and any other mission that covers MCP.

---

## 8. Progress Tracking

AI-Workshop does not have built-in progress tracking (it's a static site). Here are two lightweight approaches:

### Option A: GitHub Issues as a Checklist

1. Open a new [blank issue](https://github.com/akashtalole/AI-Workshop/issues) titled "My Progress"
2. Paste this checklist:

```markdown
## Recruit Track
- [ ] RECRUIT-00: Welcome to AI-Workshop
- [ ] RECRUIT-01: Introduction to AI & LLMs
- [ ] RECRUIT-02: Setting Up Your AI Dev Environment
- [ ] RECRUIT-03: Your First AI Application
- [ ] RECRUIT-04: Prompt Engineering Fundamentals

## Operative Track
- [ ] OPERATIVE-01: Building Conversational AI Agents
- [ ] OPERATIVE-02: Tool Use & Function Calling
- [ ] OPERATIVE-03: Retrieval-Augmented Generation
- [ ] OPERATIVE-04: Multi-Agent Orchestration
- [ ] OPERATIVE-05: AI Safety & Responsible Development

## Special Ops
- [ ] SPECIAL-OPS-01: MCP Integration
- [ ] SPECIAL-OPS-02: Claude Code Workshop
- [ ] SPECIAL-OPS-03: AI Security & Red-Teaming
```

3. Check off missions as you complete them

### Option B: Fork and Track in Your Own Repo

Fork AI-Workshop to your GitHub account. After completing each mission, commit a note file (e.g., `notes/recruit-03.md`) with what you learned and your lab output. Your fork becomes a portfolio of your AI learning.

---

## 9. Troubleshooting

### API Key Issues

| Error | Cause | Fix |
|-------|-------|-----|
| `AuthenticationError` | Wrong or expired key | Regenerate key at console.anthropic.com |
| `PermissionDeniedError` | Key lacks API access | Check your Anthropic account tier |
| Key not loading | `.env` not found | Run from the same directory as `.env` |

### Environment Issues

```bash
# Python version check
python3 --version   # Must be 3.10+

# Virtual environment check
which python   # Should point inside .venv/

# Verify SDK installation
python -c "import anthropic; print(anthropic.__version__)"

# Reload env variables
source .env   # or: from dotenv import load_dotenv; load_dotenv()
```

### Site Issues

| Problem | Fix |
|---------|-----|
| Comments not loading | Enable third-party cookies for github.com, or sign in to GitHub |
| Search not returning results | The search index builds at deploy time — wait for the latest deploy |
| Page returns 404 | Check the URL — use the navigation links in each mission, not manual URLs |
| Dark/light mode stuck | Click the 🌙/☀️ toggle in the top right, or clear localStorage |

### Getting Help

If you're stuck after checking this guide:

1. Search [GitHub Discussions](https://github.com/akashtalole/AI-Workshop/discussions) — someone may have asked the same question
2. Post in [Q&A Discussions](https://github.com/akashtalole/AI-Workshop/discussions/new?category=q-a) with your error message, Python version, and the exact step you're on
3. For code bugs in the missions, [open a bug report](https://github.com/akashtalole/AI-Workshop/issues/new?template=bug-report.yml)

---

## 10. Contributing

### Submit Feedback

The fastest way to improve the platform is submitting [mission feedback](https://github.com/akashtalole/AI-Workshop/issues/new?template=mission-feedback.yml) after completing a mission. Even a 30-second rating helps.

### Write a New Mission

1. Fork [akashtalole/AI-Workshop](https://github.com/akashtalole/AI-Workshop)
2. Create a post in `_posts/` using this front matter template:

```yaml
---
title: "TRACK-NN: Your Mission Title"
date: YYYY-MM-DD HH:MM:SS +0530
categories: [TrackName, SubCategory]
tags: [tag1, tag2, tag3]
description: "One sentence describing what learners build or learn."
toc: true
author: your-github-username
---
```

3. Follow the standard mission structure (Mission Brief → Learning Objectives → Background → Hands-On Lab → Mission Complete → Navigation)
4. Open a pull request — include a note on difficulty level and time estimate

### Report Issues

- **Broken link or code error:** [Bug report](https://github.com/akashtalole/AI-Workshop/issues/new?template=bug-report.yml)
- **Outdated content:** Comment on the mission page or open an issue
- **Mission idea:** [Feature request](https://github.com/akashtalole/AI-Workshop/issues/new?template=feature-request.yml)
