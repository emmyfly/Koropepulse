# KoropePulse

**A Hybrid Driver-Led Ledger & AI Predictive Engine for Nigeria's Informal Public Transit Networks**

Founder: Emmanuel — Noukratos Labs
Status: Concept + software prototype in progress (UNILAG pilot) · Hardware phase planned
Last updated: July 2026

---

## Table of Contents

1. [Problem Statement](#1-problem-statement)
2. [Solution Overview](#2-solution-overview)
3. [How the AI Model Works](#3-how-the-ai-model-works)
4. [MVP Scope — UNILAG Pilot (Software Only)](#4-mvp-scope--unilag-pilot-software-only)
5. [Hardware Roadmap — Phase 2](#5-hardware-roadmap--phase-2)
6. [Data Collection Plan](#6-data-collection-plan)
7. [Business Model](#7-business-model)
8. [Competitive Positioning](#8-competitive-positioning)
9. [Risks & Open Questions](#9-risks--open-questions)
10. [Roadmap](#10-roadmap)
11. [Naming & IP Notes](#11-naming--ip-notes)
12. [Repository Structure](#12-repository-structure)

---

## 1. Problem Statement

Nigeria's urban centers suffer from a highly fragmented, informal public transportation ecosystem. Major transport hubs — ranging from localized university campus parks like UNILAG to massive commercial motor parks like Oshodi, Ojota, and Nyanya — operate without centralized data.

This visibility vacuum causes real economic and structural inefficiencies:

- **Commuter Time Poverty** — passengers spend unpredictable hours standing in chaotic, unmanaged queues because they have zero visibility into vehicle arrival schedules.
- **Traffic & Gridlock Amplification** — commercial vehicles (danfos, koropes, shuttles) roam or crowd bottlenecks because they cannot gauge real-time passenger demand.
- **Union & Fleet Inefficiency** — transport operators lack the logistical data needed to optimize fleet distribution, leading to fuel waste and vehicle wear during off-peak hours.

The core problem is **data visibility**, not capacity.

---

## 2. Solution Overview

KoropePulse is a lightweight, infrastructure-light digital transit network, deliberately staged in two phases:

### Phase 1 — Software Core
- **Mobile Driver Manifest**: a one-tap PWA for drivers to broadcast location-lifecycle events ("Arrived at Park", "On the Move", "Arrived at Destination").
- **AI Predictive Transit Engine**: ingests dispatch logs, weather, rush-hour schedules, and commuter inputs to forecast demand, recommend dispatch, and generate dynamic ETAs.

### Phase 2 — Automated Sensing Hardware
- **Vehicle Motion & Distance Tracker**: a compact node per vehicle performing on-device geofence/checkpoint detection along each vehicle's fixed route, removing dependency on manual driver check-in compliance.
- **Privacy-Preserving Queue Sensor**: a park-mounted sensor estimating queue length via on-device inference — only a numeric count ever leaves the device, never an image or face.

This staging is deliberate: Phase 1 proves the software and demand model cheaply and fast. Phase 2 is the credible, fundable path to real national scale, where automated sensing removes the single largest adoption barrier — voluntary driver compliance.

---

## 3. How the AI Model Works

- **Demand Forecasting** — predicts passenger surges ahead of time (e.g., a rainy Friday rush-hour spike) from historical dispatch and queue data.
- **Intelligent Dispatching** — calculates optimal vehicle counts per hub and alerts idle or moving drivers to redirect before a supply deficit causes a bottleneck.
- **Dynamic ETA Generation** — factors time of day, check-in frequency, and historical choke points into continuously updated ETAs, rather than static timers.

**Model honesty note:** the initial model is a heuristic/regression baseline trained on limited empirical field data (manually observed BRT headways, queue counts, weather tags), augmented with synthetic data for coverage gaps. This is explicitly a validated proof-of-concept, not a production forecasting system. The roadmap toward a full time-series model depends on continuous dispatch-log access through a LAMATA/NURTW-style partnership.

---

## 4. MVP Scope — UNILAG Pilot (Software Only)

| Layer | Technology | Notes |
|---|---|---|
| Frontend | React or Flutter (PWA) | Optimized for low-end phones, minimal bandwidth |
| Real-time backend | Firebase Realtime DB / Supabase | Instant state sync between drivers and commuters |
| Predictive layer | scikit-learn or Prophet, Flask/FastAPI | Simulated + limited real BRT data; heuristic baseline |
| Data collection | Manual field observation | Headway, queue count, dwell time, weather, peak/off-peak tags at BRT terminals |

No hardware is deployed at this stage — it would contradict the low-infrastructure value proposition and cannot be responsibly built and validated within a short competition window. For any submission with a hard build deadline the software layer stands alone, with hardware presented explicitly as roadmap, not demo.

---

## 5. Hardware Roadmap — Phase 2

### 5.1 Vehicle Motion & Distance Tracker

Design goal: **clean, small, install-in-minutes** — not a breadboard-and-jumper-wire rig.

- Single custom PCB (KiCad) integrating an ESP32-class module (e.g., ESP32-WROOM, shielded/castellated — avoids routing a bare chip + external crystal), a low-cost GPS/GNSS receiver (NEO-6M class), and a LoRa transceiver for park-to-park range.
- On-device geofence logic flags arrival/departure/en-route transitions locally — raw position is *not* streamed continuously, which keeps power and radio traffic low.
- Sealed small enclosure (roughly credit-card sized), single power tap (12V accessory line → buck converter), magnetic or bracket mount. Target install time: under 5 minutes, minimal tools.
- Vehicle count at a park is derived as the count of devices currently inside that park's geofence.
- Alternative embodiment: dead-reckoning/wheel-pulse sensing to supplement GPS where signal is intermittent.

### 5.2 Privacy-Preserving Queue Sensor

- On-device inference (motion-blob segmentation or a lightweight person-detector) — no raw frame, crop, or facial region is ever transmitted, stored, or exposed downstream.
- Optional hardware-level privacy: a low-resolution or purpose-limited sensor (e.g., grayscale/thermal) that is inherently incapable of resolving facial features, as a hardware-enforced guarantee independent of software.
- Output is a scalar or coarse-bucketed count only.

**Regulatory note:** even count-only crowd sensing at a public transit hub is likely treated as data processing under Nigeria's NDPR. This should be pre-empted with an explicit data-minimization and processing-basis statement in any pitch or filing, rather than left for a judge or regulator to raise first.

---

## 6. Data Collection Plan

Given real time constraints, the data-gathering plan is deliberately narrow and high-signal rather than broad and shallow:

- **Sites**: 1–2 BRT terminals (fixed-route, semi-structured dispatch — the best available proxy for real headway/queue data without sensors).
- **Windows**: 2–3 fixed observation windows per day (morning rush, midday, evening rush) over 4–5 days.
- **Fields logged per bus**: arrival time, approximate queue count (tally), dwell time, weather flag (rain / no rain), day of week.
- **Minimum viable dataset**: ~60–100 observations is enough to fit a believable baseline (moving-average headway + simple regression) and to honestly claim the model is "trained on real field data" — a meaningful differentiator versus purely synthetic-data submissions.

---

## 7. Business Model

| Revenue stream | Description |
|---|---|
| Union/DisCo-style partnership fees | Municipal or union licensing of the dispatch/analytics platform per park or corridor |
| Advertising & sponsored placement | In-app placement for local businesses near transit hubs, shown to commuters waiting for ETAs |
| Data & analytics licensing | Aggregated, anonymized congestion/demand data licensed to city planners, LAMATA, or logistics companies |
| Hardware-as-a-service | Leased sensor nodes (vehicle tracker + queue camera) to parks/unions, recurring node + connectivity fee |

---

## 8. Competitive Positioning

- Most existing Nigerian transit-tech targets already-digitized formal fleets (BRT scheduling apps, ride-hailing). KoropePulse targets the much larger informal layer — danfo, korope, shuttle networks — that current solutions don't reach.
- The phased hardware approach avoids the common failure mode of transit-tech pilots that over-invest in infrastructure before proving driver and commuter adoption.
- Privacy-by-architecture in the queue sensor differentiates against any competing CCTV-based crowd analytics approach, on both cost and regulatory defensibility.

---

## 9. Risks & Open Questions

- **Driver adoption**: Phase 1 depends on manual check-in compliance; Phase 2 hardware is the actual fix, not an add-on, and should be framed as such.
- **Informal governance**: NURTW park chairmen currently control queue order — position KoropePulse as augmenting, not replacing, this structure.
- **Data validity**: forecasting claims must be scoped to what real (not synthetic) data currently supports, with an explicit roadmap for when full time-series claims become credible.
- **NDPR compliance**: crowd-counting cameras, even without face capture, likely require a stated legal basis for processing under Nigerian data protection law.

---

## 10. Roadmap

| Stage | Milestone |
|---|---|
| Stage 0 (current) | UNILAG closed-ecosystem software pilot — manifest + heuristic predictive layer |
| Stage 1 | Field data collection at 2–3 BRT terminals; baseline model retraining on empirical headway/queue data |
| Stage 2 | Startup-pitch-style submission — validated software pilot + phased hardware roadmap |
| Stage 3 | Pilot deployment of vehicle tracker + queue camera nodes at one real park, in partnership with a union or transit authority |
| Stage 4 | Multi-park rollout; data/analytics licensing revenue stream activated |

---

## 11. Naming & IP Notes

- Product name: **KoropePulse** — checked against web search for collisions (July 2026); no exact match found for the exact string, unlike the earlier working name ("TransitPulse"), which collided with three live products (an App Store app, an Australian transit tracker, and a generic site).
- This was a basic web-search clearance only, **not** a formal trademark register search. A proper CAC/Trademarks Registry (Nigeria) or WIPO search is recommended before public launch or registration.
- A separate `KoropePulse_Invention_Disclosure.docx` document contains a draft invention disclosure for the vehicle-tracking and privacy-preserving queue-sensing hardware, intended for review by a registered patent agent prior to filing.
- **Important**: Nigeria is a first-to-file jurisdiction with no reliable grace period. Public disclosure of the specific technical claims (via pitch decks, demos, or this repository if made public) before a provisional filing could weaken priority. This repository is kept **private** for this reason until the IP question is resolved.

---

## 12. Repository Structure

```
/docs
  KoropePulse_Business_Pitch.docx        — full business/pitch brief
  KoropePulse_Invention_Disclosure.docx  — draft patent disclosure (pre-filing, for patent agent review)
  KOROPEPULSE_WRITEUP.md                 — this document
/software                                 — (planned) PWA frontend, backend, predictive model
/hardware                                 — (planned) KiCad PCB design, firmware, enclosure files
/data                                     — (planned) field-collected BRT observation datasets
```
