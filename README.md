# KoropePulse

A hybrid driver-led ledger and AI predictive engine for Nigeria's informal public transit networks — starting with a software pilot at UNILAG, with a phased roadmap toward automated vehicle tracking and privacy-preserving queue sensing hardware.

**Status:** Concept + software prototype in progress · Private repository — not yet publicly disclosed pending patent filing review.

## Contents

- [`docs/KOROPEPULSE_WRITEUP.md`](docs/KOROPEPULSE_WRITEUP.md) — full concept write-up: problem statement, solution architecture, AI model, hardware roadmap, data plan, business model, risks, and IP notes.
- [`docs/KoropePulse_Business_Pitch.docx`](docs/KoropePulse_Business_Pitch.docx) — business/pitch brief.
- [`docs/KoropePulse_Invention_Disclosure.docx`](docs/KoropePulse_Invention_Disclosure.docx) — draft invention disclosure for patent agent review. **Do not make this repository public before a filing decision is made.**

## Structure

```
/docs      — written concept, pitch, and IP documents
/software  — (planned) PWA frontend, backend, predictive model
/hardware  — (planned) PCB design, firmware, enclosure files
/data      — (planned) field-collected transit observation datasets
```

## Quick summary

Nigeria's informal transit hubs (danfo/korope parks, campus transit like UNILAG) operate with zero centralized visibility into vehicle location or passenger demand. KoropePulse addresses this in two phases:

1. **Software core** — a one-tap driver manifest app + an AI predictive engine forecasting demand and generating dynamic ETAs, piloted in a closed campus ecosystem.
2. **Hardware phase** — automated vehicle geofence tracking and a privacy-preserving queue sensor (count-only, no imagery ever leaves the device), removing dependence on manual driver compliance.

See the full write-up in `docs/` for details.
