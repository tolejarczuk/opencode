---
name: docs
description: >
  Documentation workflow skill. Auto-identifies docs from code context,
  applies section-appropriate styles, handles update docs command in plan
  and build modes. Active when karpathy-guidelines is active.
---

## Auto-Identification Rules

Map code paths to doc sections:

| Code Path Pattern | Likely Doc Section |
|-------------------|------------------|
| `frontend/src/app/features/` | `feature-overview/` |
| `BR.Framework/WebAPI/`, `*/Controllers/` | `API Reference/` |
| `*/Hangfire/`, background jobs | `components/hangfire/` |
| `compose/`, `docker/`, CI files | `development-guides/` |
| `BR.Framework/Data/`, DB entities | `architecture/` |
| New component/service | `components/` |

## Style Matrix (Auto-Selected)

| Doc Section | Default Style | Characteristics |
|-------------|--------------|----------------|
| `API Reference` | Technical | HTTP, schemas, auth, codes |
| `feature-overview` | Simpler | User journey, value, context |
| `components` | Direct | Config, inputs, outputs |
| `architecture` | Technical | Patterns, infra, data flow |
| `development-guides` | Direct | Step-by-step, onboarding |
| `getting-started` | Simpler | Background, gentle intro |
| `troubleshooting` | Direct | Symptom -> cause -> fix |
| `contributing` | Direct | Process, checklist |

## Flows and Schemas (Plan + Build Modes)

### When
- Plan mode: while discussing a feature, before coding
- Build mode: after code is written

### How

**Flows (Sequence Diagrams):**
- Identify: API calls, service interactions, user journeys
- Ask: "Add flow diagram for [flow-name]? [y/n]"
- If yes -> generate Mermaid sequence diagram
- Embed directly in relevant doc

**Schemas (ER/Class/Architecture Diagrams):**
- Identify: Database entities, class hierarchies, architecture patterns
- Ask: "Add schema diagram for [schema-name]? [y/n]"
- If yes -> generate Mermaid ER/class/architecture diagram
- Embed directly in relevant doc

### Rules

- Always ask before generating (with proposal)
- Separate questions for flows vs schemas
- Propose specific diagram based on identified code/discussion
- Embed directly in docs (not separate files)
- Follow existing doc style and template

## `update docs` Command

### Plan Mode

```
update docs [feature-name] or update docs (uses context)

1. Parse current discussion/feature
2. Search docs/*/index.md registries
3. Identify affected docs
4. Report: "This affects: [doc1 (style)], [doc2 (style)]"
5. Ask per doc: "Style OK? [y/override: direct/simpler/technical]"
6. Remember choice for session
7. Draft or update docs
8. Show git diff -- docs/
```

### Build Mode

```
update docs [path-or-reference] or update docs (no arg)

With arg: specific doc, patch sections
Without arg: check session commits, batch update
```

## Mixed Docs Support

Update ALL affected docs at once. Apply respective default style per doc. Allow per-doc override. Show combined diff.

## Session Memory

Remember: docs updated, style choices per doc, last update docs results.

## Interaction

- Karpathy: docs follow "simplicity first" principle
- Caveman: doc output terse if caveman active