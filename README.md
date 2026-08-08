# HeatSafe Voice Copilot, data and verification workstream

My working repo from **[Dubai AI Hub] Builder Lab #3: Voice Agents Hackathon**, Dubai, 8 August 2026. Team 24.

**The team's submission repo is [ssuvorin/BUILDERS-3](https://github.com/ssuvorin/BUILDERS-3).** This repo is not the submission and does not claim to be. It holds the workstream I owned: the external data sources the agent depends on, the live verification tooling, and the defects that verification found.

## The project

**HeatSafe solves a real frontline problem: workers need fast access to the right safety procedures, company rules and live site conditions without stopping work to search through documents. The strongest initial market is UAE construction, where extreme weather, strict HSE requirements and large frontline workforces create a sharp need for verified, voice-first operational guidance at the point of work.**

The three questions users actually ask, all of which have to work hands-free:

1. **How do I do this?** — they say what they want to achieve and what they have to hand
2. **I'm part-way through and stuck. How do I get it done?** — they say what they were doing and what they originally wanted
3. **Is it safe for me to do this now?** — they say what they want to do, and ask against live conditions

Question 3 is the one the product must never answer on its own authority. Question 2 is the one currently missing a flow, which is [Finding 4](docs/FINDINGS.md).

HeatSafe Technologies (us, fictional vendor) builds voice-first operational safety copilots for frontline teams in high-risk environments. Meridian Construction LLC (the client, fictional) runs sites in the UAE.

A worker on a scaffold, hands full, asks a question out loud. The agent answers from **Meridian's own SOPs**, checks **live weather** against thresholds read out of those SOPs, names its source, and refuses anything no source covers. It advises. It never decides. Stop/go calls belong to the supervisor.

| Layer | Tool |
|---|---|
| Voice in and out | ElevenLabs Agents |
| Live web data | context.dev |
| Engineering | Devin |

## What's here

| Path | What it is |
|---|---|
| [`docs/DATA-SOURCES.md`](docs/DATA-SOURCES.md) | The source register. Every external source the agent can cite, ranked by authority, each verified live |
| [`docs/FINDINGS.md`](docs/FINDINGS.md) | Three findings. Two build defects, and what the data layer would need to sell outside the launch market |
| [`docs/SUBMISSION-REQUIREMENTS.md`](docs/SUBMISSION-REQUIREMENTS.md) | The official deliverables, transcribed from the submission desk |
| [`docs/devin-log.md`](docs/devin-log.md) | What we asked Devin, what came back, what we'd ask differently |
| [`scripts/check_sources.mjs`](scripts/check_sources.mjs) | Probes all 13 launch-market sources live, cross-checks them against each other, prints the go/no-go verdict |
| [`scripts/check_regional_sources.mjs`](scripts/check_regional_sources.mjs) | Probes 31 occupational-safety authorities across 12 markets, for the worldwide expansion case |

## The rule everything here serves

> Do not invent a figure, threshold, spec or procedure that is not in a retrieved source.

In a domain where people work at height, a confidently wrong number is worse than no answer. So every figure the agent speaks has to trace back to a named source, and the sources themselves have to be checked rather than trusted. That is what this repo is.

## Run the source check

Node 18+. No dependencies, no API keys.

```bash
node scripts/check_sources.mjs
```

Probes live conditions, UAE regulatory sources and official guidance, cross-checks the weather providers against each other, and prints what the agent should say right now against the client's thresholds.

## Credits

Team 24: Silvia Mogas, Sergei Suvorin, Sahand Sorouri, Lucy Scott Brown, Ankita Biswas.

The Meridian SOP set is Lucy Scott Brown's work. The backend, agent config and eval suite are Sergei Suvorin's. This repo covers the data layer only.

Fictional company, fictional client, fictional thresholds. Not operational safety advice.
