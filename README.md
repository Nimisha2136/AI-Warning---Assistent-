# SaafSaans 🌫️
### An AI early-warning assistant for Delhi's air

> Built for the **1M1B AI for Sustainability Virtual Internship**, in collaboration with **IBM SkillsBuild** and **AICTE**.

**[Live prototype ](https://claude.ai/artifact/XA2DtwLxaXN7iGxYs78zC2)**


---

## The problem

Delhi's AQI is published hourly by the CPCB — the data isn't missing. What's missing is the decision layer. A number like "AQI 340" doesn't tell a parent whether their child should play outside, or an asthma patient whether today is a mask day. Most residents find out the air is bad by feeling it, not by being warned.

**How might we use AI to forecast air quality deterioration in Delhi so that vulnerable residents can act *before* exposure, rather than after?**

## SDG alignment

| Goal | Role |
|---|---|
| **SDG 13** — Climate action | Primary |
| **SDG 3** — Good health and well-being | Secondary |
| **SDG 11** — Sustainable cities and communities | Secondary |

## How it works

Three AI jobs, chained together:

```
AQI history ─┐
Weather feed ─┼─► Forecast engine ─► Retrieval layer (RAG) ─► Personalisation ─► Daily brief
User profile ─┘
```

1. **Forecast** — predicts the AQI band 24–48 hours ahead from recent AQI trend, wind speed, temperature inversion, and season.
2. **Retrieval (RAG)** — grounds the forecast in real published sources: CPCB AQI category breakpoints, GRAP stage-wise restrictions, and WHO air quality guidelines. Nothing in the output is invented.
3. **Personalisation** — turns the forecast + grounded facts into a short, plain-language brief for one household, in English and Hindi.

### RAG corpus

| Source | What it grounds |
|---|---|
| CPCB AQI category breakpoints | Which band a reading falls into |
| GRAP stage-wise rules (CAQM) | What restrictions are active at that band |
| WHO air quality guidelines | An independent health-threshold check |
| CPCB / health advisory guidance | Precaution language (never medical advice) |

## Target users

- Parents of school-age children
- Elderly residents and people with asthma/COPD
- Outdoor workers (delivery riders, street vendors, construction labour)
- Schools and RWAs making institutional calls (indoor assembly, purifier use)

## Responsible AI considerations

- **Transparency** — every forecast ships with a confidence level (high/medium/low); low confidence is stated, never hidden.
- **Fairness** — CPCB monitoring stations cluster unevenly across Delhi; the system acknowledges sparser coverage in outer and informal settlements rather than implying citywide accuracy.
- **Ethics** — precaution guidance only. No diagnosis, no medication advice; serious symptoms are routed to a doctor.
- **Privacy** — health conditions and locality are sensitive inputs; only the minimum is stored, and the profile is designed to live on-device.

## Prompt workflow (for the full Granite-based version)

The prototype's rule-based logic mirrors three prompts designed for IBM Granite / any LLM with structured output:

1. **Forecast** — returns strict JSON (`aqi_24h`, `aqi_48h`, `confidence`, `key_driver`) from recent AQI + weather inputs.
2. **Ground** — answers "what precautions apply" and "what GRAP stage is active" using only retrieved passages, with citations.
3. **Advise** — combines both into a sub-120-word bilingual brief, decision-first, confidence stated honestly.


## Tech

- Prototype: single self-contained HTML/CSS/JS file, no build step, no dependencies
- Intended production stack: IBM Granite models via prompt engineering, RAG over the corpus above, IBM BOB / agentic workflow for orchestration




