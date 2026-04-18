# Part 1 — GOES Scenario Details & Discovery Notes

> **Back to:** [Part 1 — Architecture Strategy](part-1-architecture.md) · [Interview Briefing](README.md)

---

These are your **pre-engagement discovery notes** from the kickoff calls with GOES leadership. Treat them the way you would a real customer engagement brief: the numbers are your constraints, the opinions are your political landscape, and the contradictions are intentional — GOES is a real company with real internal disagreement.

Use these facts to **ground your architecture decisions**. When the panel asks *"why did you pick that?"*, your answer should reference something in this brief, not a generic cloud principle.

---

## 1. Company Snapshot

| | |
|---|---|
| **Revenue (FY25)** | ~$4.2B |
| **Business mix** | ~52% e-commerce · ~40% streaming subscriptions · ~5% live-event ticketing · ~3% payments-as-a-service |
| **Markets** | 28 countries across North America, Europe, APAC |
| **Headcount** | ~8,500 total · ~1,200 engineering · ~80 SRE/platform |
| **Ownership** | Publicly traded (NYSE) — SOX-regulated |
| **Founded** | 2004 — e-commerce first, streaming added 2016 via acquisition |

---

## 2. Business Services

### E-Commerce
- **4.5M SKUs** (physical goods, digital goods, gift cards)
- **450K orders/day** baseline · **2.8M/day** at Black Friday peak
- Average order value: ~$62
- 12 fulfilment warehouses (6 NA, 4 EU, 2 APAC) — inventory sync is a recurring pain

### Streaming
- **380K titles** (film, series, live sports rights)
- **90 PB** of master + mezzanine media storage, growing ~18%/year
- **15M paying subscribers**, 45M total monthly actives (free + ad-supported tier)
- Weekly live sports events during season — **cannot go dark** during broadcast windows

### Live-Event Ticketing
- Launched 2023 — sells tickets for concerts, sports events, and live streaming broadcasts
- ~140 major events/year (stadium-scale concerts, playoff fixtures, festival runs)
- **On-sale windows** are the hardest traffic pattern GOES handles: 90-second queues of **2M+ concurrent users** hitting a single SKU when a tour drops
- Tickets are the business unit **most affected by the velocity gap** — competitors (Ticketmaster, AXS, DICE) iterate on queueing, fraud, and resale weekly
- Tight coupling with streaming: ticket buyers often get streaming entitlements to the same event

### Payments
- Processes card-present + card-not-present transactions for GOES properties + 4 external merchant partners
- **PCI-DSS Level 1** scoped environment — handled in-house, not outsourced
- ~$18B total transaction volume annually
- Considered a strategic moat; leadership will **not** accept a third-party payments provider

---

## 3. Operational Scale

| Metric | Baseline | Peak | Notes |
|---|---|---|---|
| HTTP requests/day | 120M | 800M | Peak = Black Friday weekend |
| E-commerce peak multiplier | 1x | ~4x | Black Friday, Boxing Day, Prime-competing events |
| Ticketing on-sale spike | 1x | **~200x** for 2–5 min | Major tour on-sales — the platform's hardest traffic pattern |
| Streaming egress | 2.4 PB/month | 5x baseline | During live sports events |
| Concurrent streams | ~900K | 4.8M | Champions League final (2025) |
| Order-processing latency target | p95 < 400ms | Frequently breached at peak | Ties to inventory-sync lag |

Peak capacity sits idle **~95% of the year**. Leadership views this as the single largest waste in the current architecture.

---

## 4. Current Infrastructure

- **2 primary data centres:** Ashburn VA (primary NA) · Frankfurt (primary EU) — both owned, ~8 years old
- **2 colo POPs** for edge delivery: Singapore, São Paulo
- **~12,000 physical + virtual servers** across both DCs
- **Inter-DC connectivity:** 2× 10 Gbps IPsec VPN, with dedicated MPLS to CDN edge

### Data Stores

| System | Use | Rough Size |
|---|---|---|
| Oracle RAC | Orders, customer master, financial ledger | 340 TB |
| Cassandra | Product catalogue, session state | 180 TB |
| PostgreSQL | Payments ledger (PCI-scoped) | 95 TB |
| MongoDB | Cart, wishlist, user preferences | 60 TB |
| Ceph | Media originals + transcoded variants | 90 PB |
| ELK Stack | Observability (logs, metrics, some APM) | 2 PB hot, 8 PB warm |

### Network & Security
- Perimeter: Palo Alto firewalls + F5 load balancers
- WAF on external edge only; east-west traffic is flat
- Active-passive DR between DCs — **never actually failed over in production**
- Annual DR exercises pass on paper but have never moved real traffic

---

## 5. IT Spend Baseline

| Category | Annual | Notes |
|---|---|---|
| Hardware + refresh | ~$95M | 5-year cycle |
| Colo, power, cooling | ~$62M | |
| Software licensing | ~$88M | Oracle ~$28M of this |
| Network (MPLS, transit) | ~$41M | |
| Staffing (ops + infra) | ~$74M | |
| CDN (3rd-party) | ~$20M | |
| **Total IT operations** | **~$380M** | |

**~$85M hardware refresh is due in the next 18 months** — 40% of fleet is past vendor end-of-life. This refresh is the forcing function behind the transformation decision: leadership would rather spend the $85M on cloud migration than on another 5-year CapEx cycle.

---

## 6. Application Stack

| Layer | Technology | Notes |
|---|---|---|
| Commerce monolith | Java 11 / Spring Boot, ~4.2M LOC | Single deployable; the source of most velocity pain |
| Payments | .NET 4.8 (legacy) | PCI-scoped; modernisation paused due to risk |
| Mobile APIs | Node.js | ~2 years old; the only service shipping weekly |
| Frontends | React (web) + Angular (legacy admin) | |
| CI/CD | Jenkins + Artifactory | |
| Messaging | On-prem Apache Kafka (3 clusters) | |
| Deployment cadence | Quarterly (commerce) · weekly (mobile) | |
| Observability | Self-managed ELK + Prometheus + some Datadog | |

The commerce monolith is the **single biggest drag on velocity**. An estimated 35% of dev time is spent on merge conflicts, regression testing, and release coordination rather than feature work.

---

## 7. SLA & Availability Targets

| Service | Target | Actual (last 12 months) |
|---|---|---|
| E-commerce storefront | 99.95% (4.4h/yr downtime) | 99.91% — **missed** |
| Streaming playback | 99.99% | 99.97% — **missed during peaks** |
| Payments authorisation | 99.99% | 99.995% — met |
| Checkout-to-order latency (p95) | < 400ms | 680ms during Black Friday 2025 |

Missed SLAs in FY25 cost an estimated **$11M in credits + lost revenue**.

---

## 8. Compliance & Data Residency

- **GDPR** — EU customer data must physically reside in EU; right-to-erasure SLAs are 30 days
- **PCI-DSS Level 1** — applies to all payment flows; current scope is physically isolated in both DCs
- **SOX** — financial ledger controls; change management is audited quarterly
- **Regional data residency** — EU customer PII cannot cross borders; product catalogue is global; media can be regionally cached but masters stay in EU or NA by studio contract
- **Known gap:** APAC customer data currently replicates to Ashburn (non-compliant in some APAC jurisdictions; legal is aware and has accepted risk short-term)

---

## 9. Pain Points Driving the Transformation

In priority order from the CTO:

1. **Peak capacity is $100M+ of idle hardware for 95% of the year.** The economics are indefensible to the board.
2. **18-month lead time** to add physical DC capacity — we missed the 2024 Champions League opportunity because we couldn't scale in time.
3. **Competitor velocity gap.** Amazon and Netflix ship features weekly; we ship quarterly. We are visibly falling behind.
4. **CVE patching MTTR is 22 days** on average — security team has flagged this as a board-level risk.
5. **Inventory replication lag between DCs** causes oversells during peaks — ~$6M in cancellations + reputation damage in FY25.
6. **Ticketing on-sales are frequently embarrassing.** Two major 2025 tours collapsed under 200x load spikes; lost revenue + PR damage estimated at $14M, and a congressional inquiry was opened.
7. **Talent retention.** Engineers are leaving for cloud-native employers; we can't hire replacements who want to work on bare-metal.

---

## 10. Strategic Direction (from the CTO)

> *"We're not doing lift-and-shift. If we're going to disrupt the business, we're going to come out the other side with something worth having."*

- **Hybrid cloud** is the target — not full public cloud
- **One primary cloud provider preferred** (for operational simplicity) — multi-cloud acceptable for specific workloads (e.g., DR, data sovereignty)
- **3-year migration horizon** — board-committed
- **No net-new headcount** for the migration — must be absorbed by existing teams plus contractor burst
- **Target outcomes:**
  - 30% total IT cost reduction vs current $380M run-rate by end of year 3
  - 4x deployment velocity (quarterly → monthly at minimum for commerce monolith, ideally weekly)
  - Zero missed SLAs during peak events
  - Exit the 2027 hardware refresh cycle entirely

---

## 11. Budget & Non-Negotiables

| | |
|---|---|
| **Transformation budget** | $120M over 3 years (run-rate savings start year 2) |
| **Flag days** | **Not permitted.** Commerce + streaming must remain live throughout |
| **Live events** | No migration activity during weekly live sports broadcast windows |
| **EU data residency** | EU customer data stays in EU regions — non-negotiable |
| **Payments** | Must remain PCI-DSS Level 1 compliant wherever it lands (cloud is acceptable if architected correctly) |
| **Oracle** | Leadership is **open** to replacing Oracle but understands the risk; 18-month exit target is aspirational |
| **Observability** | Willing to replace self-managed ELK with a managed platform — cost and ops burden of the current stack is significant |

---

## 12. Known Opinions on the Team (Your Political Landscape)

These are the biases you'll encounter on the panel. Each stakeholder will push their own agenda during Q&A — plan for it.

- **Application Stack Developer** — Wants containers and Kubernetes *everywhere*. Has a prototype running on EKS already. Will push back hard on anything that looks like lift-and-shift. Doesn't fully appreciate the PCI scope implications.
- **Enterprise Architect** — Deeply sceptical of vendor lock-in. Has been burned by Oracle licensing in the past. Will ask about exit strategies, portability, and open standards. May resist managed services on principle.
- **VP Engineering** — Cost and velocity. Will ask hard questions about the $120M budget, the 30% savings target, and how deployment velocity actually gets to 4x. Doesn't care about technology religion — cares about outcomes.
- **Internal Business Stakeholder** — Black Friday 2026 is her nightmare. Will ask repeatedly about risk, rollback, and "what happens if this goes wrong during peak." She's the one who will kill the project if she loses confidence.
- **CFO (not on panel but quoted often)** — Wants OpEx over CapEx. Wants predictable monthly spend. Does not understand reserved instances vs on-demand and will need it explained.

---

*← [Back to Part 1 — Architecture Strategy](part-1-architecture.md) · [Interview Briefing](README.md)*
