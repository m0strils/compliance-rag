# NIST AI RMF — quick reference (ILLUSTRATIVE SEED — public-knowledge summary)

> This is an original, plain-language SUMMARY written for the ComplianceRAG demo
> so the retrieval pipeline has something to ground on. It is **not** the
> authoritative text. For the real framework, consult NIST AI RMF 1.0
> (NIST AI 100-1) and the accompanying Playbook and Generative AI Profile
> (NIST AI 600-1) at https://www.nist.gov/itl/ai-risk-management-framework.

## What it is
The NIST AI Risk Management Framework is a **voluntary**, U.S. framework for
managing the risks of AI systems across their lifecycle. It is not a law and
prescribes no penalties; it gives organizations a common structure and
vocabulary for building **trustworthy AI**. It is widely referenced in
enterprise governance programs and pairs naturally with the EU AI Act's
risk-based obligations.

## Characteristics of trustworthy AI
The framework describes AI that is: valid and reliable; safe; secure and
resilient; accountable and transparent; explainable and interpretable;
privacy-enhanced; and fair, with harmful bias managed. These are the *properties*
the four core functions below help you achieve and evidence.

## The four core functions
1. **Govern.** The cross-cutting culture and accountability layer. Establish
   policies, roles, and responsibilities; define risk tolerance; ensure
   oversight, documentation, and a feedback culture. Govern applies throughout —
   it is what makes the other three functions repeatable rather than ad hoc.
2. **Map.** Establish context and frame risk. Understand the system's purpose,
   its intended use and users, its dependencies, and where impacts could land.
   You cannot manage a risk you have not identified — Map surfaces them.
3. **Measure.** Analyze, assess, benchmark, and monitor. Use quantitative and
   qualitative methods to evaluate the trustworthiness characteristics — track
   metrics, test for bias, evaluate robustness and security, and monitor drift
   over time.
4. **Manage.** Act on the risks. Prioritize, respond to, and treat risks based on
   the mapped context and measured evidence; allocate resources; plan for
   incident response and recovery; and document decisions.

## The Generative AI Profile
A companion profile (NIST AI 600-1) adapts the four functions to generative AI,
enumerating GenAI-specific risks — e.g. confabulation/hallucination, dangerous
or harmful content, data privacy, information integrity, and prompt-injection /
information-security risks — and suggested actions against each.

## How this maps to a build
The four functions map cleanly onto the enterprise-readiness spine: **Govern** ->
governance + audit logging; **Map** -> threat modeling and data lineage;
**Measure** -> evaluation/observability (faithfulness, drift, cost/latency);
**Manage** -> reliability (fallbacks, incident response) and cost controls. When
ComplianceRAG cites this file, it should note that the RMF is guidance, not law,
and point to the official NIST publications for detail.
