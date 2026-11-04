# harvey

Project card maintained by Octant. Octant updates it after each task; edit it freely, and Octant will keep your changes.

## Purpose

A local autonomous cold-email agent that prospects, writes, sends and handles replies using a Claude or Codex subscription instead of API keys or paid prospecting tools.

## Users

Mostly me, dogfooding it for my own sales

## Current goal

Improve deliverability and reply/meeting rates

## Definition of done

A measurable improvement in replies or meetings booked in live use

## Stack

- Python 3.11/3.12
- FastAPI + Uvicorn (local dashboard on 127.0.0.1:5555)
- Vanilla HTML/CSS/JS dashboard (harvey/web) with vendored fonts
- SQLite via aiosqlite
- Pydantic models
- httpx, BeautifulSoup4, dnspython
- Playwright/Chromium (LinkedIn browser automation)
- aiosmtplib (SMTP/IMAP)
- Claude Code CLI (default brain provider, --safe-mode isolation)
- OpenAI Codex CLI 0.159.3 app-server (opt-in provider, macOS/Linux)
- Gmail API (OAuth), Microsoft 365/Entra OAuth, Instantly API
- Email verification/discovery: Reoon, ZeroBounce, Hunter.io
- Precleared (compliance checks), TrulyInbox (warmup)
- Discovery sources: OpenStreetMap (default), DataForSEO and others
- Optional Cloudflare Browser Rendering for training crawls
- Docker / docker-compose
- GitHub Actions

## Structure

harvey/ is the package. It holds agents/ (scout, writer, sender, handler, analyst sub-agents), collectors/ (discovery, ATS hiring, people/profile finding), integrations/ (mail providers, calendar, LinkedIn, email finder, quota, Precleared, TrulyInbox), models/ (campaign, company, conversation, prospect) and web/ (dashboard frontend). Core modules include brain.py, claude_runtime.py, codex.py, pipeline.py, gate.py, state.py, dashboard.py, cli.py and untrusted.py. prompts/ holds the agent system prompts. skills/ is the editable Markdown sales knowledge injected into prompts per agent. tests/ is a large pytest suite with fake Anthropic/Codex harnesses and fixtures. docs/ holds Microsoft 365 and Precleared guides plus screenshots. The root has harvey.yaml (main config), .env.example, the Dockerfile and docker-compose.yml.

## Commands

- run: `harvey run (heartbeat loop); harvey dashboard (WebUI at 127.0.0.1:5555); harvey setup / harvey train <url> for configuration`
- test: `python -m pytest -q (opt-in suites: HARVEY_PACKAGE_TESTS=1, HARVEY_CODEX_CLI_TESTS=1, HARVEY_CLAUDE_CLI_TESTS=1)`
- install: `python3 -m venv .venv && source .venv/bin/activate && pip install -e . && python -m playwright install chromium (dev: pip install -e ".[dev]")`

Install with pip install -e ".[dev]", test with python -m pytest -q, run with harvey dashboard or harvey run.

## Conventions

- GitHub Actions test matrix on ubuntu/macos/windows × Python 3.11/3.12 for PRs and pushes to main
- CI uses no secrets, credentials or network services; Codex is tested with scripted CLIs and localhost mocks
- CI also tests the installed wheel and the real pinned Codex CLI 0.159.3 (non-Windows)
- requirements.txt must be kept in sync with pyproject.toml dependencies
- Dependencies pinned with upper bounds against major-version breaks
- CONTRIBUTING.md is the contributor guide; the repo is public, so test data must be fictional (example.com/.test) and live test results reported as counts only
- Skills are plain Markdown mapped to agents via skill_map in harvey/brain.py
- CHANGELOG.md maintained; semantic version currently 0.3.0
- No linter or formatter config is visible in the repo

Feature branches + PRs to main, update CHANGELOG.md each PR

## Do not touch without asking

Anything on the send path, the model-isolation and prompt-injection defenses, dashboard auth, and compliance/do-not-contact logic.

## Deployment

Local only: users install from the repo or run docker compose on native Linux; no hosted deployment.

## Data sources

Yes, read aggregate counts from my local data/harvey.db

## Notes

**The repo is Inclet/harvey, but pyproject.toml's URLs point to github.com/ethanplusai/harvey. Which is canonical, and should the project URLs be updated?**
Inclet/harvey is canonical; update pyproject URLs

**The repo's CLAUDE.md is an assistant onboarding script, and it still describes Instantly as the only paid sending integration and Claude Max as the requirement, while the README and .env.example favor Gmail/SMTP/M365 and include Codex. Should CLAUDE.md be treated as out of date and brought in line with the README?**
Yes, it's stale; update it to match the README

## Decisions

- 2026-11-04 — Onboarded through the interview
no other.
