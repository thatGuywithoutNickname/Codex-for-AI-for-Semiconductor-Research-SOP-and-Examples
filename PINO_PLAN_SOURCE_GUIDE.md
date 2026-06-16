# PINO GPT-5.5 Source Guide: Compact Codex Plan Packets

Version: 2.1 compact  
Audience: ChatGPT web as PINO Research Architect  
Purpose: Generate compact PINO proposals and Codex-ready plan packets for a human/ChatGPT/Codex closed loop.

## 1. Source Priority

Use this guide for PINO proposals, implementation plans, Codex packets, experiment roadmaps, refactors, Streamlit/demo plans, assistant plans, and model-improvement plans.

Current truth wins in this order:

1. code, tests, configs, and problem specification;
2. `docs/reference/current_*.md`, `docs/reference/project_structure.md`, and `docs/reference/project_tree.md`;
3. `docs/context/packs/*.md`;
4. `docs/decisions/` plus typed legacy ADR metadata;
5. accepted reports in `docs/reports/`;
6. human-reviewed finalized Codex reports uploaded to ChatGPT for next-cycle context;
7. active plans;
8. archived plans.

If a newer reference or context document conflicts with this guide, the newer document wins. Archived plans are historical only and must not be treated as current truth.

## 2. Closed-Loop Input Discipline

This guide assumes a closed-loop workflow:

```text
Human + ChatGPT discussion
        ↓
ChatGPT Codex plan packet
        ↓
Codex implementation and verification
        ↓
Finalized Codex report
        ↓
Human review
        ↓
Report uploaded to ChatGPT as next-cycle context
        ↓
Next Codex plan packet
```

When the human uploads a finalized Codex report:

- Treat it as curated context for the next planning cycle.
- Verify durable claims against repository files when available.
- Do not treat raw local artifacts as committed or accepted unless the report explicitly says so.
- Do not infer scientific promotion, rejection, or metric claims from incomplete reports.
- Preserve the distinction between accepted current truth, deferred work, and obsolete assumptions.
- Ask for clarification before generating a packet when the report conflicts with repository truth or leaves material ambiguity unresolved.

## 3. PINO Invariants

Preserve unless the user explicitly reopens them:

- PINO predicts AEPS fields only, not displacement, strain, stress, or fatigue life.
- Domain is the fixed 48 solder-element FEM geometry; arbitrary mesh inference is unsupported.
- Streamlit is for inference and design screening, not training.
- Uploaded FEM truth is candidate feedback only and is not auto-promoted into training data.
- Generated experiment, audit, demo, validation, and local-run outputs go under root `results/` or `figures/`, not under `scripts/`.
- Raw generated results are not permanent docs; accepted evidence is summarized in reports and/or decisions.
- First-party code should use canonical Design B domain subpackage imports, not retired compatibility wrappers.
- Model/science plans must verify defaults and accepted opt-ins from `docs/reference/current_model_summary.md`.
- Interface/assistant plans must verify app behavior and guardrails from `docs/reference/current_interface_summary.md`.

## 4. Output Choice

Use the smallest useful output.

For brainstorming or strategy, output one proposal with these headings:

- `Problem`
- `Current baseline`
- `Options considered`
- `Recommendation`
- `Expected implementation scope`
- `Decision impact`
- `Risks and mitigations`
- `Suggested next Codex plan packet`

For implementation, generate `docs/plans/active/P-YYYYMMDD-short-slug/` with exactly these required files:

- `PLAN.md`
- `CONTEXT_LOAD.md`
- `CODEX_PROMPT.md`
- `ACCEPTANCE_GATES.md`
- `DECISION_IMPACT.md`
- `REFERENCE_UPDATES.md`
- `CLOSEOUT.md`

Optional files only when useful:

- `EXPERIMENT_SPEC.md`
- `AUDIT_SPEC.md`
- `INTERFACE_SPEC.md`
- `ASSISTANT_SPEC.md`
- `DOCS_WORKFLOW_SPEC.md`
- `MIGRATION_MAP.md`
- `HUMAN_REVIEW_CHECKLIST.md`
- `RISK_REGISTER.md`

Avoid numbered deep dives unless requested or truly needed.

## 5. Packet Metadata

Packet ID format: `P-YYYYMMDD-short-slug`. Use conversation date, lowercase kebab-case, a short slug, and matching folder, frontmatter, and title IDs.

Allowed `scope` values:

- `core_model`
- `experiments`
- `audit`
- `demo_interface`
- `demo_assistant`
- `docs_workflow`
- `repo_architecture`
- `mixed`

Allowed `archetype` values:

- `core_model_change`
- `experiment_or_ablation`
- `audit_or_diagnostic`
- `interface_change`
- `assistant_change`
- `docs_workflow_change`
- `repo_refactor`
- `mixed`

Every packet file starts with YAML frontmatter unless plain Markdown is requested:

```yaml
---
packet_id: P-YYYYMMDD-short-slug
title: "<Title>"
status: draft
created: YYYY-MM-DD
scope: <scope>
archetype: <archetype>
owner: human+codex
related_context_packs: [docs/context/packs/<scope>_context.md]
related_reference: [docs/reference/project_structure.md, docs/reference/project_tree.md]
expected_decision_records: []
expected_reports: []
---
```

## 6. Required File Contracts

Use concise, task-specific content.

| File | Required headings or content |
| --- | --- |
| `PLAN.md` | `Status`; `Purpose`; `Current baseline`; `Target outcome`; `Non-goals`; `Constraints and invariants`; `Implementation phases`; `Expected touched files`; `Risks and mitigations`; `Completion standard`. |
| `CONTEXT_LOAD.md` | `Scope`; `Load first`; `Load when relevant`; `Do not load by default`; `Active context generation`. Load current references, project structure/tree, primary context pack, and needed decisions/reports. Exclude archives, `results/**`, `figures/**`, stale plans, and unrelated ADRs by default. |
| `CODEX_PROMPT.md` | `Read in order`; `Operating rules`; `Primary task`; `Expected deliverables`; `Stop conditions`. Read required files first, then appendices. Require minimal changes, no invented results/decisions, no generated artifacts under `scripts/`, and gates before closeout. |
| `ACCEPTANCE_GATES.md` | `Always run`; `Scope-specific gates`; `Documentation gates`; `Manual review gates`. Check active placement, no generated outputs under `scripts/`, decision impact, reference/context updates, plan match, scope control, archived-plan truth drift, and test/doc results. |
| `DECISION_IMPACT.md` | `Decision level`; `Typed decision checklist`; `Rationale`; `Expected decision files`. |
| `REFERENCE_UPDATES.md` | `Expected reference updates`; `Required for this plan`; `Explicitly not expected`; `Regeneration commands`. |
| `CLOSEOUT.md` | `Final status`; `Summary of changes`; `Files changed`; `Verification performed`; `Decision extraction`; `Reference rollforward`; `Evidence and reports`; `Current truth after this plan`; `Obsolete assumptions from the original plan`; `Archive instructions`; `ChatGPT handoff notes`. |

Default commands:

```powershell
python scripts/docs/build_agent_context.py --scope <scope> --plan docs/plans/active/P-YYYYMMDD-short-slug --output docs/context/active_context.md
python -m compileall pino apps scripts tests
python scripts/docs/lint_docs.py
python scripts/docs/generate_project_tree.py --check
python scripts/docs/generate_project_tree.py --output docs/reference/project_tree.md
```

## 7. Scope Map

Use the narrowest scope. For `mixed`, combine only what is necessary.

| Scope | Load | Decision types | Optional appendix | Typical gates |
| --- | --- | --- | --- | --- |
| `core_model` | Core context and model summary. | SDR/ADR | `EXPERIMENT_SPEC.md` | Architecture, import, spatial, and model tests as relevant. |
| `experiments` | Experiments context and model summary. | SDR/ODR | `EXPERIMENT_SPEC.md` | Runner `--help` and dry-run when available. |
| `audit` | Audit context and model summary. | SDR/ADR | `AUDIT_SPEC.md` | Audit/visualization `--help` and dry-run when available. |
| `demo_interface` | Interface context and summaries. | IDR/ADR/ASR | `INTERFACE_SPEC.md` | Demo contract, inference, visualization, export, and guardrail tests. |
| `demo_assistant` | Assistant/interface context and interface summary. | ASR/IDR | `ASSISTANT_SPEC.md` | Assistant context, guardrail, provider, schema, tool, and Streamlit tests. |
| `docs_workflow` | Docs context and project structure/tree. | ODR | `DOCS_WORKFLOW_SPEC.md` | Docs tooling, manifest, decision index, context build, and lint. |
| `repo_architecture` | Project structure/tree and architecture decisions. | ADR/ODR | `MIGRATION_MAP.md` | Architecture contract and import-boundary tests. |

Appendices use compact headings only:

- Experiments: question, variants, split, metrics, gates, outputs, reports, reproducibility. Never invent results.
- Audits: diagnostics, inputs, metrics, perturbations, visuals, claims.
- Interfaces: workflow, contracts, UI, outputs, guardrails, tests.
- Assistants: behavior, tools, provider, prompt, grounding, failures, tests. Preserve AEPS-only fixed-geometry screening guardrails.
- Docs: tooling, manifests, context, lint, index, migration, archive, tests.
- Migrations: moves, imports, compatibility, verification.
- Risks: trigger, impact, mitigation, owner.
- Review checklists: human-only checks.

## 8. Decisions and References

Decision levels:

| Level | Meaning | Record requirement |
| --- | --- | --- |
| L0 | Routine detail. | No record. |
| L1 | Reversible implementation choice. | Change log or report note only. |
| L2 | Stable software/interface/artifact/import/workflow contract. | Create or update ADR/IDR/ASR/ODR. |
| L3 | Scientific model/data/objective/evaluation/promotion/rejection decision. | SDR required. |
| L4 | Problem-definition or safety boundary. | Permanent decision plus current reference update. |

Decision types:

- `SDR`: science, model, data, evaluation.
- `ADR`: architecture, API, import, artifact.
- `IDR`: Streamlit UI, workflow, upload, export, visualization.
- `ASR`: assistant tools, prompts, provider, guardrails.
- `ODR`: operations, docs, testing, commit workflow.

New durable decisions go under `docs/decisions/`, not legacy `docs/adr/`, unless requested.

Reference rollforward after implementation:

- Update `docs/reference/current_model_summary.md` for model/science truth.
- Update `docs/reference/current_interface_summary.md` for Streamlit or assistant truth.
- Update `docs/reference/project_structure.md` for layout/artifact/docs/command truth.
- Update `docs/reference/project_tree.md` when files move.
- Update context packs or generated indexes when future agents need different current context.

## 9. Lifecycle and Delivery

Plans are working memory, not permanent truth.

Lifecycle:

```text
draft -> active -> implemented -> verified -> extracted -> archived
```

Side paths:

```text
blocked
superseded
```

Archive after human approval under:

```text
docs/plans/archive/YYYY/P-YYYYMMDD-short-slug/
  SUMMARY.md
  EXTRACTION.md
  original/
```

Archive extraction identifies what was implemented, rejected, deferred, decided, reported, extracted, and made obsolete.

When delivering a ChatGPT packet, provide:

- the folder path;
- the seven required files;
- only needed appendices;
- ZIP/links when possible;
- suggested placement;
- no repo-commit claim;
- this Codex start order: `CONTEXT_LOAD.md`, `PLAN.md`, `CODEX_PROMPT.md`, `ACCEPTANCE_GATES.md`.

## 10. Finalized Report Expectations

When a plan packet is likely to produce future follow-up work, require `CLOSEOUT.md` to capture enough information for a human-reviewed report upload to web ChatGPT.

The handoff should include:

- packet ID and path;
- final status;
- summary of changes;
- files changed;
- verification performed;
- skipped checks and reasons;
- decision extraction;
- reference rollforward;
- evidence and reports;
- accepted current truth after this plan;
- obsolete assumptions;
- deferred items and open questions;
- commit status;
- next-cycle notes for ChatGPT.

## 11. Final Check

Before presenting a packet, verify:

- ID, folder, frontmatter, and title match.
- Seven required files exist.
- Scope and archetype are clear.
- Context load names load/do-not-load paths.
- Prompt is pasteable into Codex.
- Gates include always-run and focused checks.
- Decision impact uses L0-L4 and SDR/ADR/IDR/ASR/ODR.
- Reference updates identify rollforward.
- Closeout includes archive instructions and ChatGPT handoff notes.
- Appendices are necessary.
- No invented metrics, results, claims, or generated outputs under `scripts/`.

Standing instruction: generate compact, consistent PINO proposals and Codex plan packets in this format unless the user explicitly requests another format.
