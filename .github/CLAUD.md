# CLAUDE.md

  Project context for Claude Code, read on every `@claude` mention.

  ## Project overview

  Working repository for the official Anthropic API course. Contains
  exercises,
  examples, and experiments built on top of the Anthropic Python SDK.

  ## Stack

  - **Language:** Python 3.11+
  - **Core dependency:** `anthropic` (Python SDK) —
  https://github.com/anthropics/anthropic-sdk-python
  - **Env management:** `python-dotenv` (loads `ANTHROPIC_API_KEY` from
   `.env`)
  - **Testing:** `pytest` (when tests are added)

  ## Repo layout

  .
  ├── .github/workflows/claude.yml   # Claude Code GitHub Action
  ├── exercises/                      # Course exercises, one folder
  per module
  ├── examples/                       # Standalone runnable snippets
  ├── notebooks/                      # Jupyter notebooks (if used)
  ├── requirements.txt                # Pinned dependencies
  ├── .env.example                    # Template for required env vars
  (no secrets!)
  └── CLAUDE.md

  (Add/remove folders as the structure evolves.)

  ## Setup

  ```bash
  python -m venv .venv
  source .venv/bin/activate           # Windows: .venv\Scripts\activate
  pip install -r requirements.txt
  cp .env.example .env                # Then fill in ANTHROPIC_API_KEY

  Common commands

  - Run a script: python exercises/<module>/<file>.py
  - Run tests: pytest
  - Format: ruff format .
  - Lint: ruff check .

  Conventions

  - Models: Default to claude-sonnet-4-6 for exercises. Use
  claude-haiku-4-5-20251001 for cheap/fast iteration. Reach for
  claude-opus-4-7 only when complexity demands it.
  - Always load the key via python-dotenv — never hardcode sk-ant-...
  strings.
  - Always pass max_tokens explicitly on messages.create calls — the
  SDK does not default it.
  - Prefer client.messages.stream() over blocking create() for anything
   longer than ~200 tokens, so output appears incrementally.
  - Snake_case for Python files and identifiers; kebab-case for folders
   if needed.
  - One concept per file in exercises/ — keep examples small and
  focused.

  Anthropic SDK patterns

  Minimum viable call:

  import os
  from anthropic import Anthropic
  from dotenv import load_dotenv

  load_dotenv()

  client = Anthropic()  # Reads ANTHROPIC_API_KEY from env
  automatically

  response = client.messages.create(
      model="claude-sonnet-4-6",
      max_tokens=1024,
      messages=[{"role": "user", "content": "Hello, Claude."}],
  )

  print(response.content[0].text)

  What to avoid

  - Don't commit .env, raw API keys, or credentials of any kind.
  .gitignore must include .env.
  - Don't hardcode model names in multiple places — define MODEL =
  "claude-sonnet-4-6" once per file/module.
  - Don't catch and swallow anthropic.APIError silently; surface the
  error so course exercises are debuggable.
  - Don't add unrelated cleanup or refactors inside an exercise commit
  — keep each commit focused on one course module.

  How to use @claude in this repo

  - Ask conceptual questions: @claude what's the difference between
  system prompts and user messages?
  - Request reviews on a PR: @claude please review
  - Ask for a code fix: @claude the streaming example throws KeyError,
  can you fix it?
  - Get test coverage: @claude add pytest tests for
  examples/tool_use.py

  ---

  **Before committing, also add a `.gitignore`** (if you don't have
  one) with at minimum:

  .env
  .venv/
  pycache/
  *.pyc
  .pytest_cache/
