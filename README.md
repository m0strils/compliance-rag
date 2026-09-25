# ComplianceRAG

**A governed *knowledge runtime* for AI governance itself — a RAG assistant that
answers "what does the framework say about X / is my AI system compliant?"
grounded in the *public* texts: the EU AI Act, the NIST AI RMF, and the OWASP
LLM Top 10.**

It is deliberately meta: the AI that cites the real frameworks the AI industry
is governed by. And it is built as a *course* — every capability is a numbered
lesson, and the runtime itself can narrate what it's doing (`--explain`), so the
repo doubles as a teaching aid.

> Built as a teaching-first portfolio piece: a working demonstration of
> production-minded, grounded RAG — retrieval, grounding, and audit as one
> integrated system, not a chunk-and-pray demo. This is a personal
> learning-by-building project (see the honesty note at the bottom).

## Positioning (2026): a knowledge runtime, not a RAG demo
"Naive RAG" — chunk-and-pray, retrieve-then-generate — is on its way out. The
durable 2026 pattern is the **knowledge runtime**: retrieval + verification +
reasoning + access-control + audit as *integrated operations*, with the judgment
to route between plain retrieval, long-context, and **agentic retrieval** (the
model inside the loop, deciding when it has enough context). ComplianceRAG's MVP
implements the grounded, audited, cited-to-source core; agentic retrieval,
citation-to-clause, evals, and retrieval/long-context routing are the roadmap
(and are marked honestly as such in the scorecard below).

## Why this exists
Two reasons, and they reinforce each other:

1. **The subject matter is the governance frameworks the industry runs on.**
   100% public data, universally relatable, and directly on-topic for anyone who
   has to reason about AI risk. Ask it "what are the EU AI Act risk tiers?" or
   "which OWASP LLM risk is prompt injection?" and it answers from summarized
   source notes, with citations.
2. **It's a course.** Most RAG tutorials stop at "retrieve → generate."
   Enterprises can't, and neither does this. The hard parts are first-class and
   each is a lesson:

- **Security** — an input guardrail with prompt-injection heuristics + input
  limits (OWASP **LLM01**), and instruction/data separation in the prompt itself
  (retrieved text is data, never instructions).
- **Groundedness** — an output guardrail: answers must cite retrieved sources or
  they are visibly flagged `⚠️ UNGROUNDED`. For a compliance assistant this is
  the whole point — an uncited claim about "what the law requires" is a liability.
- **Governance** — a JSONL audit log per request: question, model, sources,
  token usage, cost estimate, latency, blocked/grounded status.
- **Cost control** — per-request cost estimation and a running dashboard
  (`--audit`): requests, blocked, ungrounded, total spend, p50 latency.
- **Teaching mode** — `--explain` narrates every stage as it fires, turning a
  run into a lesson.

The controls live in `enterprise.py`, deliberately stdlib-only and small enough
to read line-by-line. (It's a standalone copy — this repo imports nothing from
its sibling projects.)

## Quickstart

Prereqs: Python 3.12+ and [uv](https://docs.astral.sh/uv/) (or plain `venv`).

```bash
uv venv && source .venv/bin/activate
uv pip install -r requirements.txt

cp .env.example .env   # add your ANTHROPIC_API_KEY (console.anthropic.com)

python compliancerag.py --reindex                                  # index docs/
python compliancerag.py --ask "What are the EU AI Act risk tiers?"
python compliancerag.py --explain --ask "Which OWASP LLM risk is prompt injection?"
python compliancerag.py                                            # interactive
python compliancerag.py --audit                                    # governance/cost summary
```

Embeddings run locally (Chroma's default model) — the only external call is
generation (Claude). The `docs/` files are **illustrative, original
public-knowledge summaries** of each framework, clearly marked as such; they are
not the authoritative texts. Drop in your own notes and re-index anytime.

### Teaching mode (`--explain`)
`--explain` narrates the pipeline to stderr as it runs — guardrail → retrieve →
ground → audit — so you can *see* the enterprise controls fire:

```
  · guardrail: input passed (OWASP LLM01 injection heuristics + length limit)
  · retrieve: top-4 chunks from ['eu-ai-act.md']
  · ground: context assembled as DATA (instruction/data separation), prompt built
  · generate: calling engine=api model=claude-sonnet-5 ...
  · guardrail: output grounding ✅ cited
  · audit: recorded (in=812 out=240 tok, 1900ms)
```

Retrieval and grounding are shown even without an API key — only the generate
step needs one.

### Engines (cost control)
Generation is pluggable — an enterprise pattern (model/provider routing) in miniature:

```bash
python compliancerag.py --ask "..."                      # default: Anthropic SDK (metered API)
python compliancerag.py --engine claude-code --ask "..." # headless Claude Code on a Pro/Max subscription ($0 marginal)
```

`--engine api` gives exact token/cost telemetry and is the standard production
pattern. `--engine claude-code` shells out to `claude -p`, billing nothing extra
if you have a Claude subscription (token counts are estimated; shares your
subscription's usage limits). The audit log records which engine served each
request. Set a default with `COMPLIANCERAG_ENGINE=claude-code` in `.env`.

## Architecture (MVP)

```
docs/*.md (framework summaries) ──chunk──> Chroma (local vectors)
                                              │  top-k retrieve
user ──input guardrail (LLM01)──────────────> Claude (context = data, not instructions)
                                              │
                                      output guardrail (grounding / citation check)
                                              │
                                      audit log (tokens · cost · latency · grounded)

          --explain narrates every arrow above as it fires.
```

## Enterprise-readiness scorecard
✅ built · 🟡 partial · ⬜ roadmap

| Dimension | Control | Status |
|---|---|---|
| Security | input/output guardrails, injection defense (LLM01), instruction/data separation | ✅ |
| Governance | per-request audit log w/ cost + grounding; secrets hygiene; cited-to-source | ✅ |
| Evaluation | golden dataset + faithfulness gate (RAGAS) | ⬜ |
| Observability | latency/cost in audit log | 🟡 |
| Reliability | graceful refusal over hallucination; retries/fallback routing | 🟡 |
| Cost | per-request estimate + running total; model routing | 🟡 |
| Deployment | IaC, CI/CD, AWS Bedrock in-VPC | ⬜ |
| Context/Memory | agentic retrieval, citation-to-clause, retrieval/long-context routing, agentic memory | ⬜ |

The scorecard is honest on purpose. The grounded/audited/secure core is built
(✅); observability, reliability, and cost show partial (🟡 — real signals in the
audit log, but not yet dashboards/routing/tracing); eval, deployment, and the
agentic context/memory layer are roadmap (⬜).

## Roadmap
1. **Citation-to-clause:** retrieve at the clause/section granularity so answers
   cite "EU AI Act, high-risk obligations" rather than just a filename.
2. **Agentic retrieval:** LangGraph agent that plans → retrieves → checks "enough
   context?" → answers, and routes between retrieve / long-context / iterate (the
   knowledge-runtime loop). An MCP tool server ("look up NIST RMF function X").
3. **Quality gate:** a RAGAS eval harness on a golden set of governance Q&A;
   block regressions in CI. (Evaluation ⬜ → ✅.)
4. **Observability:** Arize Phoenix tracing over every retrieve/generate call.
5. **Hardening:** classifier-grade guardrails (e.g., Bedrock Guardrails), model
   routing, caching.
6. **Cloud:** port generation to AWS Bedrock (data stays in-VPC).

## Course / lessons
This repo is structured as a course. See [`TEACHING.md`](./TEACHING.md) for the
index that maps each lesson to the 8 enterprise-readiness dimensions. Lesson 1,
[`lessons/01-grounded-compliance-qa.md`](./lessons/01-grounded-compliance-qa.md),
walks the MVP build end to end.

## Honesty note
This is a **personal learning / portfolio project** — learning-by-building, not
a product and not a compliance service. The `docs/` are original
public-knowledge *summaries* written for the demo; they are **not** the
authoritative legal or standards texts, and nothing here is legal advice. For any
real obligation, consult the official sources: the EU AI Act (Regulation (EU)
2024/1689), the NIST AI RMF (NIST AI 100-1), and the OWASP Top 10 for LLM
Applications (genai.owasp.org). No overclaiming: the scorecard above marks what
is actually built versus what is roadmap.

## License / data
MIT. All `docs/` content is original public-knowledge summary material — no
copyrighted text is reproduced, and no proprietary or non-public data is
included.
