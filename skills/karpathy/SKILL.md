---
name: karpathy-guidelines
description: >
  Behavioral guidelines to reduce common LLM coding mistakes. Think before coding,
  simplicity first, surgical changes, goal-driven execution. Auto-triggers in plan mode
  and build mode. Respects caveman toggle commands.
---

Behavioral guidelines to reduce common LLM coding mistakes, derived from Andrej Karpathy's observations on LLM coding pitfalls.

Tradeoff: These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## Principles

### 1. Think Before Coding

Don't assume. Don't hide confusion. Surface tradeoffs.

Before implementing:

- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

### 2. Simplicity First

Minimum code that solves the problem. Nothing speculative.

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

### 3. Surgical Changes

Touch only what you must. Clean up only your own mess.

When editing existing code:

- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:

- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

### 4. Goal-Driven Execution

Define success criteria. Loop until verified.

Transform tasks into verifiable goals:

- "Add validation" -> "Write tests for invalid inputs, then make them pass"
- "Fix the bug" -> "Write a test that reproduces it, then make it pass"
- "Refactor X" -> "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
1. [Step] -> verify: [check]
2. [Step] -> verify: [check]
3. [Step] -> verify: [check]

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

### 5. Comments and Documentation

Prefer readable code over comments. Comment intent and edge cases, not obvious steps.

#### C# Backend

**Required on all `public` and `internal` APIs:**

```csharp
/// <summary>
/// Clear description of what this does and why it exists.
/// </summary>
public class Foo { }

/// <summary>
/// Description of the method's purpose and outcome.
/// </summary>
/// <param name="id">What this parameter represents (not the type)</param>
/// <returns>What the method returns and when</returns>
/// <exception cref="NotFoundException">When and why this is thrown</exception>
public async Task<RobotDto> GetRobot(int id) { }
```

**Swagger/OpenAPI on Controllers (public endpoints only):**

```csharp
[HttpGet("{serialNumber}")]
[SwaggerOperation(
    Summary = "Get Robot",
    Description = "Returns full robot details including status and battery")]
[SwaggerResponse(StatusCodes.Status200OK, "Robot found", typeof(RobotDto))]
[SwaggerResponse(StatusCodes.Status404NotFound, "Robot not found")]
[ProducesResponseType(typeof(RobotDto), StatusCodes.Status200OK)]
public async Task<ActionResult<RobotDto>> Get([FromRoute] string serialNumber) { }
```

Rules:
- `/// <summary>` — Always on public/internal classes, methods, properties
- `<param>` — Required if method has parameters
- `<returns>` — Required if method returns value
- `<exception>` — Required if method throws
- `[ProducesResponseType]` + `[SwaggerOperation]` — Required on all public controller actions
- `[SwaggerResponseExample]` — Only for complex DTOs where example clarifies usage
- Private methods — Comment only if complex logic or non-obvious intent

#### TypeScript / Angular Frontend

```typescript
/**
 * Schedules a best-effort access-token refresh before the JWT expires.
 * Does NOT read refresh token (cookie-only in browsers).
 */
@Injectable({ providedIn: 'root' })
export class AccessTokenRefreshService {
    /**
     * Start or re-start the refresh timer based on the current access token.
     */
    public StartOrReschedule(): void { }
}
```

Rules:
- `/** ... */` on exported classes, services, public methods
- Explain *why*, not *what* (code shows what)
- `@param`, `@returns` — Optional unless complex parameters

#### HTML Templates

Minimal comments only:
```html
<!-- Robot Status Card -->
<div class="status-card">...</div>
```

#### General Principles

- Prefer readable code over comments
- Comment intent, edge cases, and "why" decisions
- Do not comment obvious steps
- Update comments when code changes

### 6. Flow Validation (Complex Features)

**Trigger:**
- Auto-detect: Multi-layer changes, >5 files, new APIs, DB migrations, auth changes, external integrations
- User request: "show me the flow", "validate the flow", "what's the plan?"

**Before implementing:**
1. State implementation steps with verification checks
2. Show data flow diagram (Mermaid sequence)
3. List files to change
4. Ask: "Approve this flow? [y/n]"

**If rejected:**
- "What to change?"
- WAIT for user input
- Update flow based on feedback
- Re-present for approval
- WAIT again
- Never proceed until approved

## Modes

### Plan Mode (Auto-trigger)

Activated by: `enter plan mode`

Apply all principles. Think fully, present tradeoffs, define plan with verification steps.

Communication: Terser if caveman active. Full detail if caveman off.

### Build Mode (Auto-trigger)

Activated by: `enter build mode`, `build this`, `implement this`

Apply karpathy principles with build workflow.

#### Workflow

**.NET Projects:**
1. Code (surgical, minimal - Principle #3)
2. Ask user: "Write tests? [y/n]"
3. If yes -> ask: "New project or existing? Location?"
4. If yes -> write NUnit tests (verify success criteria - Principle #4)
5. Show test results
6. Show diff for review
7. Skip `dotnet build` entirely

**.Angular Projects:**
1. Code (surgical, minimal - Principle #3)
2. Skip tests (for now)
3. Show diff for review
4. Skip `pnpm run build` entirely

#### Mode Transitions

- `exit build mode` -> Return to Plan Mode (both karpathy + caveman on)
- `exit plan mode` / `stop plan mode` -> Normal mode (both off)

#### Git Operations

- User handles ALL git operations: commit, push, merge, branch, etc.
- AI NEVER runs: `git commit`, `git push`, `git merge`, `git branch`, etc.
- AI only shows diffs (e.g., `git diff -- docs/`) and writes files.
- User decides when and what to commit. AI proposes nothing.

### Documentation

For docs workflow, see `docs` skill. Available in plan and build modes.

## Toggle Commands

| Command | Effect |
|---------|--------|
| `karpathy only` | Disable caveman, keep karpathy in current mode |
| `caveman only` | Disable karpathy, keep caveman in current mode |
| `both on` / `enable both` | Re-enable both in current mode |

## Interaction with Caveman Skill

Karpathy guides WHAT to think/do. Caveman shapes HOW to communicate.

- Think fully per Karpathy (assumptions, tradeoffs, verification).
- Output terse per caveman when caveman is active.
- When caveman disabled, communicate in normal detailed style.
- When karpathy disabled, caveman stays but without behavioral constraints.