# AGENTS.md

Instructions for coding agents working in this repo. `CLAUDE.md` points here, so this file is the single source of truth.

## Project

JSON Better Editor: a planned Flutter desktop JSON editor. Requirements live in `docs/requirements-checklist.md`.

## How to report back to the user

Every message you send back for the user to read falls into exactly one of these three groups. Start the message with its label.

1. **`[DECISION]`**: the user has to answer a question from you. Ask it clearly, and recommend an answer.
2. **`[INFORM]`**: the user must read this, but doesn't need to reply. Use it for results, changes and anything they need to know.
3. **No label**: a log of what you did. The user may read it or skip it, so never put a question or something they must know in an unlabelled message.

If a message holds both a question and information, label it `[DECISION]`.

## Agent skills

The skills in `.claude/skills/` come from [mattpocock/skills](https://github.com/mattpocock/skills). See `.claude/skills/README-mattpocock-skills.md`.

### Issue tracker

Issues and specs live in this repo's GitHub Issues, managed with the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

The five default labels: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: one `GLOSSARY.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.
