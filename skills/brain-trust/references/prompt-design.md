# Prompt design

For when the Editor is producing a prompt rather than prose.

## Element order

Order is itself an effect. Place these in this sequence:

1. Opening user turn
2. **Task context** — the role to adopt and the overall objective. Strongest at the very top.
3. Tone (omit unless it matters)
4. Detailed instructions and rules — put the permission to say "I don't know" here
5. **Examples** — wrapped in `<example>` tags, covering edge cases as well as the ordinary case
6. The input to be processed — each part in its own tag, conversation history included
7. **Restatement of the immediate task** — in a long prompt, say again at the end what it is doing
8. Thinking cue — for multi-step work, "think before answering", directly after 7
9. **Output format** — specify it at the end, not the top
10. Advanced: chaining, tool use, retrieval

What matters is not that there are ten elements. It is the position of **2, 5, 7 and 9**:

- Context at the top.
- Reminder and format at the bottom.
- Examples outweigh description — a worked edge case does more than a paragraph explaining the rule.

The recurring mistake is putting the output format at the top, where it competes with the task context, instead of at the bottom where it is the last thing read.

## Attention markers in Chinese-language prompts

Marker strength is not uniform, and the pattern differs from English-language prompting advice.

| Marker | Weight |
| --- | --- |
| **bold**, `——`, `【】`, `#`, `@` | reliably draws attention |
| `（）`, `[]` | read as an aside; low weight |

Consequence: **a constraint inside parentheses in a Chinese prompt is frequently treated as optional**, because the parenthesis itself signals secondary information. Anything that must hold goes in `【】` or bold. Never in brackets.

## The weighting rule

> **The later a principle appears in a list, the less weight it carries.**

This is a different level from element order above — that is position in the document, this is position within a list. They compound: a rule in parentheses at the bottom of a long list is, in practice, absent.

## Writing instructions that hold

Two rules that apply to any instruction set, prompt or specification:

**Be concrete, not general.** A pinned value holds. A stated principle invites interpretation, and every reader interprets differently. Where a general rule seems more elegant than an explicit list, prefer the list.

**Declare completeness.** A document that says "this list is not exhaustive" — or implies it by saying a detail is handled elsewhere — instructs the reader to fill the gap, and each reader fills it differently. If a list is complete, say so in the document: *the items listed are the complete set; do not add items not listed.*

**Scope every rule.** A rule without a stated boundary will be applied outside it. State what it covers and, where there is an adjacent case it might be mistaken for, state what it does not.
