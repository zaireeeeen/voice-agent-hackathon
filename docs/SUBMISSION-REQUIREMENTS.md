# Submission requirements — from the official desk

Source: `dubai-ai-hackathon.replit.app` · read 8 Aug 2026, 10:15

## The deadline

**Saturday 8 August, 2:30 PM GST.** Build window **8:30 AM – 2:30 PM**.
Repeat submissions welcome — the most recent one is the one they review.

## Four things they need

**01. A repository they can open**
Public, or accessible without them asking. **All commits must fall inside the build window.**

**02. `README.md` at the root**
Someone who has never seen the project can install, configure, run it. Setup that actually works (install, env vars, run command, clean machine) · architecture overview, what talks to what · what it does, in our words.

**03. `TECH-SPEC.md` at the root**
One page, five sections:
1. **Problem** — who, what pain, why voice
2. **Architecture** — data flow, diagram or description
3. **Tool rationale** — why ElevenLabs / context.dev / Devin, specifically
4. **Feasibility** — how it was scoped to 6 hours
5. **Extensibility** — what v2 looks like

**04. A demo video on Loom**
Two to three minutes. Share link set to **"anyone with the link."**

> "We check your spec against your code. Claiming streaming, real-time data, or an agent loop that isn't there costs more than not claiming it. Write what you built."

## The six video questions — answer all six

| # | Question | Guide |
|---|---|---|
| 1 | What problem does it solve, and who is it for? | ~30 sec |
| 2 | **Live, working demo** — a real voice conversation **including a live web-data fetch through context.dev.** Say what data it pulls, when, and from where | ~2 min |
| 3 | Why is live web data essential? Finish: *"Our project would fundamentally break without live web data because…"* Say whether it handles data that changes mid-conversation | ~30 sec |
| 4 | Beyond text-to-speech, what does the agent do on its own? Autonomous logic, which ElevenLabs features, how the personality and voice were designed | ~45 sec |
| 5 | What makes the approach novel? The use case, data source, or combination not seen before | ~30 sec |
| 6 | Hardest problem solved, and how | ~30 sec |

## The submission form

1. **Team number** — 24
2. **Repository URL**
3. **Demo video URL**
4. **Notes** — known issues, setup quirks, anything judges shouldn't miss
5. Four confirmation checkboxes: README at root · TECH-SPEC at root · repo + Loom accessible · all code written in today's window

## What this forces on the idea

Question 2 and question 3 are not optional. The idea **must genuinely require live web data**, and the video **must show context.dev fetching it live**. Any idea that works fine on static knowledge fails two of six questions and the "novel" one too.

Ankita owns: the ElevenLabs agent persona (Q4), `README.md`, `TECH-SPEC.md`, the Loom, `docs/devin-log.md`, and the clock.
