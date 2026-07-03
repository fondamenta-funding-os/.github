# Fondamenta Funding OS

**Fondamenta** is an AI-assisted platform for discovering, structuring, validating, and evaluating public funding opportunities. It transforms fragmented official sources into a traceable, operational workflow:

> **Pipeline:** Source → Acquisition → Normalization → AI Extraction → Evidence Linking → Human Validation → Internal Catalog → Assessment

Designed for speed and reliability, Fondamenta aims to rapidly answer three critical operational questions for any funding call:
1. **Is it relevant?**
2. **Is it economically interesting?**
3. **Are the timeline and requirements realistic for a credible application?**

---

## Architecture & Boundaries

The platform follows a lean, multi-repository architecture with strict separation of concerns.

> **Core Rule:** Connectors acquire and normalize data. The AI engine interprets it. The backend owns the canonical state. The frontend exclusively consumes backend APIs.

* **`project-plan`**: Planning, architecture (ADRs), product decisions, and cross-repo coordination.
* **`fondamenta-frontend`**: Operator and client-facing interfaces. *Rule: Consumes backend APIs only; no direct access to staging or AI layers.*
* **`fondamenta-backend`**: Canonical system of record and API orchestrator. Owns users, permissions, validation states, evidence records, audit logs, and the published catalog.
* **`fondamenta-source-connectors`**: Source acquisition layer. Owns API/scraper ingestion, document normalization, and raw staging data. *Rule: Does not own canonical product truth.*
* **`fondamenta-ai-engine`**: AI interpretation layer. Owns structured extraction, confidence scoring, evidence linking, and LLM adapters. *Rule: AI output is strictly draft data until human-validated.*
* **`.github`**: Organization-level defaults, templates, and policies.

---

## Core Product Principles

* **Traceability First:** Every critical field (e.g., deadlines, grant percentages, aid intensity, eligible beneficiaries, required documentation, ATECO constraints) must be strictly linked to source evidence.
* **Human-in-the-Loop:** AI extracts, classifies, and summarizes, but it does not replace validation. Responsible users must review and approve critical fields before they become operational data.
* **Provider-Agnostic AI:** Avoid vendor lock-in. Implement LLM adapters and ensure prompts, models, token usage, and evaluation outcomes are versioned and observable.
* **Lean Evolution:** Prove the complete vertical slice (Ingestion → Normalization → AI → Backend → Validation → Catalog) before expanding features.

---

## Engineering & Governance Standards

### Development & Stack
* **Python Standards:** Python 3.12, `uv` package manager, `src/` layout, strict typing (`mypy`), `ruff` for linting/formatting, and `pytest` (with offline testability). No external API keys required for CI smoke tests.
* **Repo Requirements:** Standardized `README`, `.gitignore`, `.env.example`, pre-commit hooks, CI workflows, MkDocs, contribution guidelines, and `AGENTS.md`/`CLAUDE.md`.

### Governance
* **Tracking:** Product delivery is managed in **Linear**. Every repo must define its Linear initiative/project, ownership, and strict service boundaries.
* **Decisions:** Architectural choices must be recorded as ADRs and linked in documentation.

### Security & Privacy
* **Secrets:** Zero secrets in code, docs, tests, or commits. Use `.env.example` for keys. Real credentials belong in a secret manager.
* **Data Privacy:** Public funding docs may go to approved external LLMs. **Internal/anonymized client data (startups, partner profiles) must never be sent to external LLMs without explicit approval.**
* **Auditability:** All validation actions must be auditable. Production requires access control, backups, and monitoring.

---

## Status

**Phase:** Transitioning from architecture and PoC planning to the implementation of the first MVP vertical slice. Priority is proving end-to-end acquisition, AI interpretation, human validation, and catalog publication with full evidence traceability.
