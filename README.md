# opencode-config

Personal [OpenCode](https://opencode.ai) configuration — custom skills and modes for better AI-assisted coding.

## What's Inside

| File | Type | Description |
|------|------|-------------|
| `skills/karpathy/SKILL.md` | Skill | Coding discipline guidelines (think first, simplicity, surgical changes, goal-driven) |
| `skills/caveman/SKILL.md` | Skill | Ultra-terse communication mode (~75% token reduction) |
| `modes/karpathy-build.md` | Mode | Karpathy guidelines as an active mode |
| `modes/caveman-plan.md` | Mode | Caveman style with write/edit/bash disabled — read-only planning mode |

## Installation

```bash
# Clone into your opencode config directory
git clone https://github.com/tolejarczuk/opencode.git ~/.config/opencode
```

## Usage

OpenCode auto-detects skills and modes from `~/.config/opencode`.

- **Skills** are loaded automatically when the context matches their description.
- **Modes** can be activated manually or assigned to agents.

### Karpathy Skill/Mode

Biases the agent toward caution and simplicity:

- State assumptions explicitly
- Minimum viable code — no speculative features
- Surgical edits only — don't refactor unrelated code
- Define verifiable success criteria before acting

### Caveman Skill/Mode

`caveman-plan` mode disables write/edit/bash tools — use it when you want analysis or planning without side effects.

## License

MIT
