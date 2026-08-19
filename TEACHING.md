# Teaching ComplianceRAG — the course index

ComplianceRAG is built to be *taught*, not just run. The premise is meta on
purpose: you learn to build a grounded, governed RAG system by building one whose
subject matter **is** the governance frameworks (EU AI Act, NIST AI RMF, OWASP
LLM Top 10). The commit history is the curriculum; each lesson adds one
capability and explains *what* we add, *why* an enterprise needs it, and *which*
framework clause it maps to.

If you only do one thing: run the MVP with the teaching flag and watch the
enterprise controls announce themselves.

```bash
python compliancerag.py --reindex
python compliancerag.py --explain --ask "What are the EU AI Act risk tiers?"
```

## The spine: 8 enterprise-readiness dimensions
Every serious AI project in this portfolio is built and scored against the same
8 dimensions. The point of the framework is honesty — "documented-only" and
"roadmap" are legitimate answers, and saying so is more credible than a wall of
green checks.

1. **Security** — OWASP LLM Top 10: guardrails, injection defense, least-privilege.
2. **Governance** — audit logging, versioning, PII handling, cited-to-source.
3. **Evaluation** — golden set + faithfulness/groundedness gate as a CI check.
4. **Observability** — tracing, cost/latency, drift.
5. **Reliability** — retries/timeouts/fallback, graceful refusal, idempotency.
6. **Cost** — model routing, caching, token budgets.
7. **Deployment** — IaC, CI/CD, secrets management, Bedrock/VPC.
8. **Context & Memory** — retrieval routing, agentic memory, progressive disclosure.

## Lessons -> dimensions

| Lesson | Teaches | Primary dimensions | Framework hooks |
|---|---|---|---|
| [01 — Grounded compliance Q&A](./lessons/01-grounded-compliance-qa.md) | The MVP: chunk → embed → Chroma → retrieve → ground → audit, plus `--explain` teaching mode | Security, Governance, Observability (partial), Reliability (partial) | OWASP **LLM01** (injection), **LLM09** (misinformation/grounding); NIST **Govern/Measure**; EU AI Act logging & human-oversight themes |
| 02 — Citation-to-clause *(roadmap)* | Retrieve and cite at clause/section granularity | Governance, Context/Memory | EU AI Act article/annex structure |
| 03 — Evals as a release gate *(roadmap)* | A RAGAS golden set + faithfulness gate in CI | Evaluation | NIST **Measure**; OWASP **LLM09** |
| 04 — Agentic retrieval *(roadmap)* | Plan → retrieve → "enough context?" → answer; retrieve vs long-context routing | Context/Memory, Reliability | NIST **Map/Manage** |
| 05 — Observability *(roadmap)* | Phoenix tracing over every retrieve/generate call | Observability | NIST **Measure** |

Roadmap lessons are listed so the shape of the course is visible from day one;
they are written as each capability lands. See the README scorecard for the
honest built-vs-roadmap status.

## How to teach with `--explain`
The `--explain` flag narrates each stage to stderr — guardrail → retrieve →
ground → audit. It's the single most useful teaching tool in the repo: learners
*see* that retrieved content is treated as data (instruction/data separation),
that grounding is checked before an answer is trusted, and that every request is
audited. Retrieval and grounding narrate even without an API key, so the lesson
runs offline; only the generate step needs a key.
