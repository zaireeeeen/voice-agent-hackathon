# Voice Agent (Hackathon)

A voice-first AI agent that knows something real about the world, right now.

Built at **[Dubai AI Hub] Builder Lab #3: Voice Agents Hackathon** — Dubai, 8 August 2026.

## The stack

| Layer | Tool | What it does here |
|---|---|---|
| Voice in / voice out | **ElevenLabs Agents** | Speech-to-text, turn-taking, text-to-speech |
| Live world knowledge | **Context.dev** | Reads the live web and returns clean, LLM-ready text |
| Engineering | **Devin** (Cognition) | Writes the code and opens the pull requests |

## Team 24

Silvia Mogas · Ankita Biswas · Sergei Suvorin · Sahand Sorouri · Lucy Scott Brown

## What it does

*To be filled in once the idea is locked at 9:45am.*

## Running it locally

```bash
cp .env.example .env   # add your own keys
```

Keys are never committed. See `.env.example` for what's needed.

## How we steered the tools

Every task given to Devin, and what came back, is logged in [`docs/devin-log.md`](docs/devin-log.md).
