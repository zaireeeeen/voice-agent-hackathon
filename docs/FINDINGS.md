# Findings

Two defects, found by verifying assumptions rather than accepting them. Both reproducible from this repo or [ssuvorin/BUILDERS-3](https://github.com/ssuvorin/BUILDERS-3).

Found 8 August 2026 during the build window. Recorded here because the reasoning is the interesting part, not the fix.

---

## Finding 1: the weather source cannot answer the question the policy asks

**Where:** `app/weather.py` in the team repo, fetching `https://wttr.in/{location}?format=j1`.

**The policy says:**

> Gusts count. If gusts exceed the limit for the activity, treat the limit as exceeded even if the sustained wind is below it.

**The source does not publish gusts.** Reading the response keys directly, `current_condition` contains `windspeedKmph` and `windspeedMiles` and no gust field of any kind.

The extraction schema asks context.dev for `wind_gust_mph`. The page has no such number. So the field returns null, or it gets inferred from surrounding text. Inferred is the worse outcome: a plausible gust figure that no source published, feeding a stop/go call about work at height. That is precisely the invented figure the project's own rules forbid.

**And the two candidate sources disagree materially.** Same site, same minute:

| | Open-Meteo | wttr.in | Difference |
|---|---|---|---|
| Temperature | 43.3 C | 37 C | **6.3 C** |
| Wind | 6.6 km/h | 12 km/h | 5.4 km/h |
| Gusts | 21.9 km/h | not provided | n/a |

6.3 C decides an outcome, because the client's policy turns at 45 C. One source puts the site 1.7 C from the threshold. The other puts it 8 C clear. Two different safety answers from two sources both described as live.

**Fix:** Open-Meteo as primary, still fetched through context.dev so the tooling story is unchanged. It returns gusts and apparent temperature, needs no API key, and has no rate limit at this volume. Keep wttr.in as fallback, name the source used in the spoken answer, and if the two disagree by more than 3 C say so aloud rather than silently picking one.

**The general lesson:** "live data" is not one property. A source can be live, free, fast and still structurally unable to answer your question. Check the fields against the rules before choosing the provider.

---

## Finding 2: better documents, silently broken retrieval

Two sets of client SOPs existed in parallel, with different numbers, and only one of them was wired in.

| | Set A | Set B |
|---|---|---|
| Files | `MC-POL-014` | `MER-SOP-014`, `MER-SOP-021`, `MER-SC-003` |
| Location | in `demo-data/`, parsed by the code | project root, not in the repo |
| Work at height | 30 km/h | 17 mph restricted, 22 mph suspended |
| Sheet materials | 20 km/h | 15 mph, any height |
| Heat | sign-off 45 C, stop 50 C | elevated band 42 to 45 C, stop above 45 C |
| Visibility | suspend below 100 m | no work above 6 m below 1,000 m, stop below 500 m |

Set B is the better documentation: tiered bands rather than single limits, escalation contacts, an internal work fallback list, and deliberate coverage gaps designed for the refusal evals. The demo script quoted Set B's numbers. The running agent would have spoken Set A's.

**The part that would have bitten:** swapping Set B in does not produce an error. It produces silence.

The threshold extractor matches a table row whose **second cell begins with a digit and a unit**. Set B's rows read `Below 17 mph (27 km/h)`. The cell begins with a word.

Running the actual regex against all three files:

| File | Thresholds extracted |
|---|---|
| MC-POL-014 | **3** |
| MER-SOP-021 | **0** |
| MER-SOP-014 | **0** |

With zero thresholds loaded, threshold matching returns nothing and every weather question answers *"No wind threshold found in the loaded SOPs."* No exception, no failed test, no log line that looks wrong. Just an assistant that has quietly stopped knowing anything, in the demo, on camera.

Two further gaps if Set B is adopted: banded thresholds cannot be represented by a one-limit-per-activity model, and the verdict logic evaluates wind only, so Set B's heat band goes unenforced.

**The general lesson:** a document swap is a code change when the code parses the document. "Just drop the better files in" is the most dangerous kind of late edit, because it looks like content and behaves like a deployment.

---

## Method

Neither finding came from reading the code and reasoning about it. Both came from running the thing and reading what actually came back: the provider's raw response keys, and the real regex against the real files. The reasoning-only version of each conclusion was available hours earlier and would have been wrong in the details that mattered.

`scripts/check_sources.mjs` exists so this stays cheap to repeat. It re-probes every source, cross-checks the providers against each other, and prints the live verdict in about 15 seconds.
