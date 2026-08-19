# EU AI Act — quick reference (ILLUSTRATIVE SEED — public-knowledge summary)

> This is an original, plain-language SUMMARY written for the ComplianceRAG demo
> so the retrieval pipeline has something to ground on. It is **not** the
> authoritative text and is not legal advice. For any real obligation, consult
> the official source: Regulation (EU) 2024/1689 (the EU AI Act) and EU
> Commission guidance at https://digital-strategy.ec.europa.eu.

## What it is
The EU AI Act is the first broad, horizontal law regulating artificial
intelligence. It takes a **risk-based** approach: the obligations scale with how
much risk an AI system poses to health, safety, and fundamental rights. It
applies extraterritorially — providers and deployers outside the EU are in scope
if their AI output is used in the EU.

## The four risk tiers
1. **Unacceptable risk — prohibited.** Practices banned outright, e.g. social
   scoring by public authorities, manipulative or exploitative techniques,
   untargeted scraping of facial images, and most real-time remote biometric
   identification in public spaces (narrow law-enforcement exceptions).
2. **High risk — heavily regulated (but allowed).** Systems used in areas such
   as biometrics, critical infrastructure, education, employment/HR, access to
   essential services, law enforcement, migration, and the justice system.
   Obligations include a risk-management system, data governance, technical
   documentation, logging/traceability, human oversight, and accuracy /
   robustness / cybersecurity. Conformity assessment is required before market.
3. **Limited risk — transparency obligations.** Systems that interact with people
   or generate content must disclose the AI. Users should know they are talking
   to a bot; AI-generated or manipulated media (deepfakes) must be labelled.
4. **Minimal risk — unregulated.** The vast majority of AI (spam filters, game
   AI, etc.). No new obligations; voluntary codes of conduct encouraged.

## General-purpose AI (GPAI) / foundation models
A separate track. All GPAI providers owe baseline transparency and a copyright
policy. Models deemed to carry **systemic risk** (very capable, broad-impact
models) owe extra duties: model evaluation, adversarial testing, systemic-risk
assessment and mitigation, and serious-incident reporting.

## Timeline (phased application)
- **Aug 1, 2024** — the Act enters into force.
- **Feb 2, 2025** — prohibitions (unacceptable-risk practices) apply; AI-literacy
  duties begin.
- **Aug 2, 2025** — GPAI provider obligations and governance bodies apply.
- **Aug 2, 2026** — the bulk of high-risk obligations apply.
- **Aug 2, 2027** — obligations for high-risk AI embedded in regulated products.

## Governance & penalties
Enforced by national authorities coordinated by a new EU **AI Office** and AI
Board. Fines are tiered and steep — up to the greater of a fixed cap or a
percentage of global annual turnover, highest for prohibited-practice breaches.

## How this maps to a build
High-risk obligations (risk management, data governance, logging, human
oversight, accuracy/robustness) line up directly with the enterprise-readiness
dimensions: governance, observability, evaluation, and reliability. When
ComplianceRAG cites this file, it should point the reader to the official
Regulation for the binding wording.
