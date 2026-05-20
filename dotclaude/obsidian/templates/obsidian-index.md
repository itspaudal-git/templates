---
type: index
tags: [index, all-prompts, all-memories]
---

# dotclaude Second Brain

Your personal reference library of prompts, memories, and knowledge synced from dotclaude.

## Prompts by Domain

- [[Prompts/Backend]]
- [[Prompts/Frontend]]
- [[Prompts/Testing]]
- [[Prompts/Debugging]]
- [[Prompts/Documentation]]

## Recent Prompts

```dataview
table skill, tags, updated
from "Prompts"
where type = "prompt"
sort updated descending
limit 10
```

## By Skill Type

- `/tdd` prompts — Test-driven development
- `/refactor` prompts — Safe refactoring
- `/debug-fix` prompts — Systematic debugging
- `/ship` prompts — Shipping workflows
- `/explain` prompts — Code explanations

### Browse by Skill

```dataview
table topic, updated
from "Prompts"
where skill = "/tdd"
sort updated descending
```

(Change `"/tdd"` to `/refactor`, `/debug-fix`, etc.)

## Recent Memories

```dataview
table scope, tags, updated
from "Memory"
where type = "memory"
sort updated descending
limit 10
```

## Graph View

Use **Graph View** (Cmd+G) to visualize connections between prompts and memories.

## Monthly Review

- Check [monthly-sync-checklist.md](../monthly-sync-checklist.md)
- Archive unused prompts
- Update connections based on new patterns
- Plan new prompts needed

## All Tags

#backend #frontend #testing #debugging #api #database #performance

## Search Tips

- `tag:#api` — Find all API-related prompts
- `type:prompt` — Show only prompts
- `type:memory` — Show only memories
- `updated >= 2026-05-01` — Recent changes

## Templates

- [Prompt Template](templates/prompt-template.md)
- [Memory Template](templates/memory-template.md)
