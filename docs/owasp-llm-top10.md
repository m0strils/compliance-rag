# OWASP LLM Top 10 — quick reference (ILLUSTRATIVE SEED — public-knowledge summary)

> This is an original, plain-language SUMMARY written for the ComplianceRAG demo
> so the retrieval pipeline has something to ground on. It is **not** the
> authoritative text. For the real list, definitions, and mitigations, consult
> the OWASP Top 10 for LLM Applications (2025) at
> https://genai.owasp.org.

## What it is
The OWASP Top 10 for LLM Applications is a community-driven list of the most
critical security risks specific to applications built on large language models.
It is the security counterpart to OWASP's classic web Top 10, and it is the
vocabulary most enterprises use to threat-model GenAI systems. IDs are of the
form `LLM01`..`LLM10`.

## The list (2025 edition — one line each)
- **LLM01 — Prompt Injection.** Crafted input (direct or hidden in retrieved
  content) overrides the model's instructions or intended behavior.
- **LLM02 — Sensitive Information Disclosure.** The model leaks PII, secrets,
  proprietary data, or system-prompt contents in its output.
- **LLM03 — Supply Chain.** Compromised or untrusted models, datasets, adapters,
  or dependencies introduce vulnerabilities.
- **LLM04 — Data and Model Poisoning.** Tampered training, fine-tuning, or
  embedded/RAG data manipulates model behavior or creates backdoors.
- **LLM05 — Improper Output Handling.** Model output is trusted downstream without
  validation/encoding, enabling XSS, SQL injection, SSRF, or code execution.
- **LLM06 — Excessive Agency.** The system grants the model too much autonomy,
  permission, or functionality, so a bad decision causes real-world harm.
- **LLM07 — System Prompt Leakage.** Sensitive instructions or secrets placed in
  the system prompt are exposed or relied on for security they cannot provide.
- **LLM08 — Vector and Embedding Weaknesses.** Weaknesses in how embeddings/vector
  stores are generated, stored, or retrieved (e.g. RAG poisoning, cross-tenant
  leakage, inversion).
- **LLM09 — Misinformation.** The model produces false or misleading output
  (hallucination/confabulation) that users over-trust.
- **LLM10 — Unbounded Consumption.** Uncontrolled inference load enabling
  denial-of-service, denial-of-wallet, or model-extraction attacks.

## Defense-in-depth (how these get mitigated)
No single control is enough. Typical layers: input validation and injection
heuristics (LLM01), instruction/data separation so retrieved content is never
executed as instructions (LLM01/LLM08), output validation/encoding before any
downstream sink (LLM05), least-privilege and human-in-the-loop for actions
(LLM06), grounding + citation checks against misinformation (LLM09), secrets
kept out of prompts and logs (LLM02/LLM07), and rate/budget limits (LLM10).

## How this maps to a build
ComplianceRAG's own `enterprise.py` implements several of these: the input
guardrail targets LLM01, instruction/data separation defends LLM01/LLM08, and
the output grounding guardrail addresses LLM09. That is the meta-payoff — the
assistant that explains the Top 10 is itself built against it. When ComplianceRAG
cites this file, it should point to genai.owasp.org for the current definitions
and mitigations.
