# My Brain Trust

*[English](README.md) ｜ [繁體中文](README.zh-TW.md)*

A five-role deliberation framework for AI agents, packaged as an [Agent Skill](https://agentskills.io/specification).

One assistant is split into five roles with separate jobs and, critically, **separate output shapes**: a Detective who verifies and labels confidence, a Coach who handles interpersonal situations, an Advisor who decides and states the cost, a Skeptic who attacks the plan before reality does, and an Editor who owns anything another person will read.

Works unmodified in Claude Code, Codex CLI, Gemini CLI, Cursor, Copilot and other tools that implement the Agent Skills open standard.

## Install

Copy the `skills/brain-trust/` directory into your agent's skills directory:

```bash
# per-user
cp -r skills/brain-trust ~/.agents/skills/

# or committed to a repository, for a team
cp -r skills/brain-trust .agents/skills/
```

Claude Code also reads `~/.claude/skills/`.

## The design principle

The framework exists because of one finding from adversarial testing: **role separation fails at the format layer before it fails at the reasoning layer.**

Running a five-role system on a free-tier model under load produces three failures that arrive mixed together but have different causes — a lower-priority role speaking out of turn, the roles' voices converging until the output reads as one person wearing five labels, and the role labels and structure degrading turn over turn.

The intuitive fix does not work. A global instruction — *"each role's voice must be clearly distinct; roles must not imitate one another"* — had no measurable effect on voice convergence. It reads as a strong instruction and does nothing, because it tells the model what to avoid and leaves it to infer what to do instead.

What worked was a per-role output format table: a concrete target the model can check itself against, turn by turn, without inference. What fixed the structural decay was changing the label specification so that **each role labels itself as it speaks, rather than the response listing its participants up front.**

The same pattern showed up from the opposite direction in a separate test, where one specification document was handed to two independent coding agents and the disagreement between their outputs was used to measure how much the document left to interpretation. Divergence went from 91.2% to 0.0% over four rounds. Pinning concrete values worked. Stating general principles made it worse — a line saying a detail was handled elsewhere effectively announced that the document was incomplete, and both readers filled the gap differently.

> **A concrete, checkable table beats an abstract prohibition.** In both tests, the instruction that failed was the one a human reader would have understood correctly. An instruction that states what to do is checkable; a description of a way of thinking is not.

## What is in it

| File | Contents |
| --- | --- |
| `skills/brain-trust/SKILL.md` | The core rules, the response format table, the five roles, evidence discipline |
| `skills/brain-trust/references/roles.md` | Working instructions for each role |
| `skills/brain-trust/references/prompt-design.md` | Element ordering and attention markers for prompt construction |
| `*.zh-TW.md` | Traditional Chinese versions — written as originals, not translated |

The Chinese files are not a localisation afterthought. `prompt-design` documents something specific to Chinese-language prompting: marker strength is not uniform, and a constraint written inside parentheses is frequently treated as optional, because the parenthesis itself signals secondary information. Most prompting guidance is written from English and does not cover this.

## Compatibility

Conforms to the Agent Skills open standard: `SKILL.md` with `name` and `description` frontmatter, Markdown body, optional `references/`. No agent-specific extensions are used, so it behaves identically across conforming tools.

Validate with:

```bash
npx skills-ref validate ./skills/brain-trust
```

## License

MIT. See `LICENSE`.
