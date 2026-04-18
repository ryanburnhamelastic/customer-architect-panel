# Part 1: Architectural Strategy for Cloud Transformation

> **Duration:** 30–45 minutes presenting · aim to wrap within 45 min to leave time for Part 2
>
> **Back to:** [Interview Briefing](README.md)

---

## Treat This Like a Customer Interaction

This is not a whiteboard exercise — **treat the panel as if they are the actual GOES leadership team** in a real engagement. That means:

- **Open with a brief recap** of your understanding of the problem before diving into solution
- **Anchor your recommendations in GOES's actual business facts** — the numbers, constraints, and internal politics in the [Scenario Details](part-1-scenario-details.md) are what you're designing against
- **Tailor your delivery** to who's in the room (developer, architect, VP, business stakeholder — each hears different things)
- **Invite questions and pushback** — a great CA engagement is a conversation, not a monologue
- **Close with clear next steps** — what follows this meeting? Workshops, POC scope, discovery deep-dives, timeline. Leaving without next steps means the customer doesn't know what to do Monday morning

The panel is evaluating whether you'd be the kind of architect they'd trust to lead their own customers through a decision like this.

---

## Scenario

- **Your Role:** Cloud Consultant
- **Client:** GOES — Global, Online E-Commerce and Streaming Content Provider
- **Current State:** On-premises (multiple physical data centers), self-managed
- **Strategic Direction:** Transitioning to a hybrid cloud environment

> **📄 [Scenario Details & Discovery Notes](part-1-scenario-details.md)** — concrete numbers, tech stack, budget, and constraints from the consultant's kickoff with GOES leadership. **Read this before designing your architecture** — it's what the panel will expect you to reference.

---

## Panel Stakeholders (Role-Play)

1. Application Stack Developer
2. Enterprise Architect
3. VP Engineering
4. Internal Business Stakeholder

---

## Objectives

Choose a cloud provider of your preference and guide the panel through the architectural transformation required.

- Design an architecture using **cloud-native patterns** that supports transitioning existing on-premise applications and infrastructure
- Address challenges of ensuring seamless operation in a **hybrid environment**
- Identify and discuss **potential risks** of moving to the cloud and propose mitigation strategies
- Highlight **opportunities** the cloud provides over their self-managed on-premise setup
- Demonstrate understanding of key transition considerations: **cost, security, scalability, and data management**

> **Note:** This segment does not require Elastic to be included. The focus is on your overall architectural understanding, migration strategy, and ability to identify risks and opportunities. Go both high-level and technically deep.

---

## Key Considerations

### Size and Scale
Large multinational business with operations across multiple continents. Millions of active users generating substantial data daily.

### Data Privacy and Compliance
Operates in regions with strict data privacy regulations (e.g., **GDPR in Europe**). Data storage and processing decisions must reflect these constraints.

### Complexity of Services
Offers multiple services in parallel:
- Online shopping
- Content streaming
- Online payment processing

### Peak Periods
Experiences significant traffic spikes (e.g., **Black Friday, Christmas**). Architecture must handle these loads without service disruption.

### Business Continuity
High availability and resiliency are non-negotiable. Significant downtime directly translates to substantial revenue loss.

### Innovation Mindset
Culture of frequent experimentation. Architecture must support **rapid changes and deployments**.

---

## Current State Architecture

![GOES Current State Architecture](diagrams/goes-current-state-architecture.svg)

The current environment spans two physical data centers (North America and East) connected via bi-directional VPN replication. Each DC runs web tiers, API tiers, databases, and PCI-compliant payment processors behind firewalls and load balancers.

---

## Deliverable

Prepare **diagrams** to visualize your architectural design. These can represent:
- A step-by-step transformation process
- Various aspects of the final (target) architecture

> **Template:** Download the [Elastic-branded PowerPoint template](presentations/goes-cloud-transformation-template.pptx) — 4 pre-structured slides with Elastic branding ready to fill in.

### Close With Clear Next Steps

End your presentation with **concrete next steps** — what would you propose happens after this meeting? Be specific. For example:

- A follow-up **discovery workshop** on payments + PCI scope (90 min, technical audience)
- A **proof-of-concept** on the ticketing on-sale spike pattern (4-week scope, clear exit criteria)
- A **target-state architecture deep-dive** with the enterprise architect
- **Cost modelling** against the $120M transformation budget with finance
- A **risk register** reviewed with the business stakeholder before Black Friday 2026

The panel wants to see that you think beyond the meeting — a good customer engagement ends with the customer knowing exactly what happens Monday morning.

---

*← [Back to Interview Briefing](README.md)*
