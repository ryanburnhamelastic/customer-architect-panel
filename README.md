# Customer Architect Panel — Interview Briefing

> **Format:** 90-minute panel interview with 3–4 Elastic employees — **target 75 minutes of content** to leave time for a real conversation at the end
>
> **Structure:** Two parts — architectural strategy + hands-on Elastic Stack
>
> **Mindset:** Treat this as a **real customer interaction**, not an academic exercise. The panel is evaluating whether you'd be the architect they'd trust to walk into their own customer's boardroom. Anchor recommendations in facts, tailor your delivery to the audience, and **close every conversation with clear next steps**.

---

## Interview Structure

| Part | Topic | Target Duration |
|------|-------|----------|
| [Part 1 — Architecture Strategy](part-1-architecture.md) | Cloud transformation design for a fictional enterprise client | ~30 min |
| Part 2 — Elastic Stack Exercise | Hands-on data ingestion and analysis *(pick one track below)* | ~45 min |
| Panel Q&A + wrap-up | Discussion, questions, feedback | ~15 min |

> ⏱️ **Time budget:** You have 90 minutes total. **Aim to deliver Parts 1 and 2 in 75 minutes** so there's room for conversation at the end — this is a panel, not a lecture.

### Choose One Track for Part 2

| Track | File |
|-------|------|
| 📊 Observability | [part-2-observability.md](part-2-observability.md) |
| 🔍 Search & AI | [part-2-search.md](part-2-search.md) |
| 🛡️ Security Operations | [part-2-security.md](part-2-security.md) |
| ⚡ ESRally Benchmarking | [part-2-esrally.md](part-2-esrally.md) |

---

## Quick Start

1. Read this page in full
2. [Create an Elastic Cloud trial](https://cloud.elastic.co/) — save your `elastic` password and Cloud ID
3. Click through to [Part 1](part-1-architecture.md) and your chosen Part 2 track
4. **Do your setup before the interview** — image pulls, model deployments, and API key configuration all take time

---

## Repo Contents

| File | Description |
|------|-------------|
| [part-1-architecture.md](part-1-architecture.md) | GOES scenario, stakeholders, objectives, current-state diagram, deliverable |
| [part-1-ae-handoff.md](part-1-ae-handoff.md) | Internal AE prep notes — relationship history, stakeholder dynamics, competitive context |
| [part-1-press-release.md](part-1-press-release.md) | GOES public announcement of the transformation programme |
| [part-1-discovery-call.md](part-1-discovery-call.md) | Edited transcript of the initial discovery session with GOES leadership |
| [part-1-news-article.md](part-1-news-article.md) | External news coverage of the GOES ticketing platform failure |
| [part-2-observability.md](part-2-observability.md) | Host metrics, Apache log ingest, OTel Demo Shop |
| [part-2-search.md](part-2-search.md) | ELSER semantic search, RAG, AI Agent Builder |
| [part-2-security.md](part-2-security.md) | Osquery, Security AI Assistant, Automated Response |
| [part-2-esrally.md](part-2-esrally.md) | ESRally benchmarking against a remote cluster |
| [presentations/goes-cloud-transformation-template.pptx](presentations/goes-cloud-transformation-template.pptx) | Elastic-branded 4-slide PowerPoint template for Part 1 |
| [diagrams/goes-current-state-architecture.svg](diagrams/goes-current-state-architecture.svg) | GOES current-state on-premises architecture diagram |
| [sample-data/apache_access.log.gz](sample-data/apache_access.log.gz) | Apache access log — 2M lines, Apr 1–17 2026 (39 MB gzip) |
| [sample-data/product-catalog.json](sample-data/product-catalog.json) | 500-record e-commerce product catalogue for the Search & AI exercise |
| [logstash/pipeline.conf](logstash/pipeline.conf) | Logstash pipeline template for Apache log ingest |
| [otel/collector-config.yml](otel/collector-config.yml) | OpenTelemetry Collector config routing telemetry to Elastic Cloud |
| [otel/docker-compose.override.yml](otel/docker-compose.override.yml) | Docker Compose override for the OTel Demo Shop |

---

## Elastic Cloud Setup

1. Go to [cloud.elastic.co](https://cloud.elastic.co/) and start a free 14-day trial
2. Create a new deployment — the defaults are fine
3. **Save immediately:** `elastic` user password and your **Cloud ID** (shown once on creation)
4. Your Cloud ID is visible later under **Deployment → Manage → Cloud ID**

> **Security track only:** You need a **Security-type Serverless project** (or a deployment with Elastic Security enabled) — this is separate from the standard deployment used in other exercises.

---

## General Notes

- **Target 75 minutes of delivery** across Parts 1 + 2 — leave the remaining 15 minutes for panel Q&A and wrap-up
- **Close every part with next steps** — what follows this meeting? Workshops, POCs, deep-dives, timelines. Don't leave the panel wondering what happens Monday morning
- **Choose ONE exercise** for Part 2 — you will not have time for more than one in the 45-minute window
- **Within your exercise**, sections marked *Intermediate* and *Advanced* represent two levels of depth — your interviewer will indicate which is expected, or you may choose based on your background
- **Documentation may be imperfect** — working through ambiguity is part of what the panel is evaluating
- **Prepare in advance:** OTel Demo image pulls (~3 GB), ELSER model deployment (5–10 min), and LLM API key setup should all be done the day before your interview
- **Elastic Cloud trials expire after 14 days** — ensure your deployment is active the morning of your interview

---

*Impress us — The Elastic Team*
