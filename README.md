# AI-Workshop

> **Learn AI by Doing** — A mission-based AI education platform

[![GitHub Pages](https://github.com/akashtalole/AI-Workshop/actions/workflows/pages-deploy.yml/badge.svg)](https://github.com/akashtalole/AI-Workshop/actions/workflows/pages-deploy.yml)

**Live Platform:** [akashtalole.github.io/AI-Workshop](https://akashtalole.github.io/AI-Workshop/)

---

## Platform Structure

AI-Workshop uses a progressive mission system modeled after special operations training:

| Track | Difficulty | Missions | Description |
|-------|-----------|---------|-------------|
| **Recruit** | `•` | 5 | Foundational AI — APIs, prompt engineering, first applications |
| **Operative** | `••` | 5 | Agents, tool use, RAG, multi-agent orchestration, AI safety |
| **Commander** | `•••` | Coming soon | Production AI systems and architecture |
| **Special Ops** | Standalone | 3 | MCP, Claude Code, AI Security — no prerequisites |

## Mission List

### Recruit Track `•`
- RECRUIT-00: Welcome to AI-Workshop
- RECRUIT-01: Introduction to AI & LLMs
- RECRUIT-02: Setting Up Your AI Dev Environment
- RECRUIT-03: Your First AI Application
- RECRUIT-04: Prompt Engineering Fundamentals

### Operative Track `••`
- OPERATIVE-01: Building Conversational AI Agents
- OPERATIVE-02: Tool Use & Function Calling
- OPERATIVE-03: Retrieval-Augmented Generation (RAG)
- OPERATIVE-04: Multi-Agent Orchestration
- OPERATIVE-05: AI Safety & Responsible Development

### Special Ops
- SPECIAL-OPS-01: Model Context Protocol (MCP) Integration
- SPECIAL-OPS-02: Claude Code Workshop
- SPECIAL-OPS-03: AI Security & Red-Teaming

## Tech Stack

- **Theme:** [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) (`jekyll-theme-chirpy ~> 7.3`)
- **Hosting:** GitHub Pages via GitHub Actions
- **Comments:** [Giscus](https://giscus.app) (GitHub Discussions)
- **CI/CD:** GitHub Actions (auto-deploy on push to `main`)

## Running Locally

```bash
# Install Ruby dependencies
bundle install

# Start local server
bundle exec jekyll serve --livereload

# Browse to http://localhost:4000/AI-Workshop/
```

## GitHub Setup (after first push)

1. **Enable GitHub Pages:** Settings → Pages → Source: **GitHub Actions**
2. **Enable Discussions:** Settings → Features → check **Discussions**
3. **Configure Giscus:** Get `repo_id` and `category_id` from [giscus.app](https://giscus.app), update `_config.yml`
4. **Create GitHub Project:** Add "Mission Roadmap" board for content planning

## Contributing

1. Fork the repository
2. Create a mission post in `_posts/` following the template in any existing mission
3. Use the correct category: `[Recruit, Foundations]`, `[Operative, Advanced]`, `[Special-Ops, Tools]`, etc.
4. Open a pull request

See [CONTRIBUTING](https://github.com/akashtalole/AI-Workshop/issues/new?template=feature-request.yml) to suggest new missions.

## Feedback

- **Mission feedback:** [Open a feedback issue](https://github.com/akashtalole/AI-Workshop/issues/new?template=mission-feedback.yml)
- **Bug reports:** [Report a bug](https://github.com/akashtalole/AI-Workshop/issues/new?template=bug-report.yml)
- **Questions:** [GitHub Discussions](https://github.com/akashtalole/AI-Workshop/discussions)

## License

[MIT License](LICENSE) — © 2026 Akash Talole
