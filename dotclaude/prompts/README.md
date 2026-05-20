# Prompt Library

Your personal library of prompts you've refined with Claude. Store, organize, and reuse them across projects and conversations. Optionally sync to Obsidian for your "second brain".

## What Goes Here

- Prompts that worked well and you want to reuse
- Rephrased versions of your initial broken English attempts
- Variations you discover for different contexts
- Task templates (TDD prompts, code review templates, etc.)

---

## Organization Strategies

### Strategy 1: By Domain (Recommended for growing collections)

```
prompts/
├── README.md
├── backend/
│   ├── api-design.md
│   ├── database-migrations.md
│   └── auth-flows.md
├── frontend/
│   ├── component-design.md
│   ├── styling-patterns.md
│   └── performance.md
├── testing/
│   ├── tdd-template.md
│   ├── test-coverage.md
│   └── edge-cases.md
├── debugging/
│   ├── memory-leaks.md
│   ├── race-conditions.md
│   └── integration-issues.md
└── documentation/
    ├── api-docs.md
    └── readme-template.md
```

### Strategy 2: By Skill Type

```
prompts/
├── README.md
├── tdd/
│   ├── feature-template.md
│   ├── refactor-template.md
│   └── debugging-template.md
├── refactoring/
│   ├── extract-function.md
│   ├── rename-module.md
│   └── break-cycle.md
├── review/
│   ├── code-review.md
│   ├── security-review.md
│   └── performance-review.md
└── design/
    ├── api-design.md
    ├── db-schema.md
    └── ui-component.md
```

### Strategy 3: By Project (Recommended if you manage multiple projects)

```
prompts/
├── README.md
├── project-a/
│   ├── setup.md
│   ├── common-tasks.md
│   └── debugging.md
├── project-b/
│   ├── setup.md
│   ├── common-tasks.md
│   └── debugging.md
└── cross-project/
    ├── typescript.md
    ├── react.md
    └── testing.md
```

---

## Prompt File Format

Each prompt file should have a simple structure:

```markdown
---
topic: "API Design"
skill: "/debug-fix"  # or /tdd, /refactor, /ship, etc. (optional)
tags: ["backend", "validation", "error-handling"]
created: 2026-05-19
updated: 2026-05-19
---

# Clear Title

## Original Broken English (Optional)

> What you actually wrote initially

## Refined Prompt

The improved version that works well with Claude.

```
your prompt here
```

## Context

- When to use this
- What it does
- Expected output

## Variations

### Variation 1: For debugging

```
your variation prompt
```

### Variation 2: For refactoring

```
another variation
```

## Notes

- Any gotchas or tips
- Common follow-ups
```

---

## How to Use Your Prompts

### 1. Save a Prompt

After a successful conversation with Claude:

```
@claude Remember: I want to save this prompt to /dotclaude/prompts/

Create a file [domain]/[name].md with:
- Original broken version
- Refined version
- When to use it
```

Claude will create/organize the file for you.

### 2. Reference Later

In a new conversation:

```
Use this prompt template:

[Copy/paste your saved prompt]

Now adapt it for [your new task]
```

### 3. Organize Over Time

Every month:
1. Move prompts that rarely work to an `_archive/` folder
2. Consolidate similar prompts into variations
3. Update tags for better discovery

---

## Starting Template

Use this as a quick-start for any prompt you save:

```markdown
---
topic: "[What this is about]"
tags: ["tag1", "tag2"]
---

# [Descriptive Title]

## When to Use

[Describe the situation where this prompt works well]

## The Prompt

```
[Your refined prompt text]
```

## Example Output

[What you expect Claude to produce]

## Tips

- [Gotcha 1]
- [Gotcha 2]
```

---

## Examples (Create your own!)

Look in `./examples/` for sample prompts once you start building your library. Remove example files when your library grows.

---

## Tips for Growing Your Prompt Library

✅ **DO:**
- Save prompts that worked well
- Update old prompts when you discover better versions
- Add context about when/why it works
- Use consistent file names (kebab-case: `api-design.md`)
- Tag prompts by domain and skill

❌ **DON'T:**
- Save one-off prompts you'll never reuse
- Keep vague prompt names (`test1.md`, `thing.md`)
- Mix multiple unrelated prompts in one file
- Forget to update when you refine something

---

## Integration with Claude

### Use `/explain` to understand a saved prompt

```
/explain prompts/backend/api-design.md
```

Claude will break down why the prompt works.

### Generate a new prompt from patterns

```
I have several prompts in prompts/testing/. Generate a new testing prompt based on their patterns.
```

### Generate a new prompt from patterns

```
I have several prompts in prompts/testing/. Generate a new testing prompt based on their patterns.
```

### Search your prompts

You can grep or search in your editor:
```bash
grep -r "api" prompts/
grep -r "debugging" prompts/
```

---

## Sync to Obsidian

Use your prompt library as the foundation of your "second brain" by syncing to Obsidian.

### One-Time Setup

1. **Copy prompts to Obsidian**
   ```bash
   cp -r .claude/prompts/* ./obsidian/Prompts/
   ```

2. **Add frontmatter** (see [../obsidian/setup.md](../obsidian/setup.md))
   ```yaml
   ---
   type: prompt
   topic: "API Validation"
   tags: [backend, api]
   dotclaude-path: ".claude/prompts/backend/api-validation.md"
   ---
   ```

3. **Create backlinks** in Obsidian to connect related prompts

### Keep in Sync

- **New prompt?** Tell Claude to save it and export to `./obsidian/Prompts/`
- **Updated prompt?** Update both `.claude/prompts/` and `./obsidian/`
- **Monthly cleanup?** Archive old prompts and remove from Obsidian

### Obsidian Workflow

See [../obsidian/setup.md](../obsidian/setup.md) for:
- Detailed sync instructions
- Note templates with proper frontmatter
- Obsidian plugins that help (Dataview, Templater, etc.)
- Monthly sync workflow

---

## Getting Started

1. **Create your first folder** based on your main project domain
2. **Save one prompt** from your most recent successful conversation
3. **Organize as you grow** — reorganize monthly as patterns emerge
4. **Sync to Obsidian** (optional) — build your "second brain" once you have 5+ prompts

No pressure to perfect the structure upfront. Let it evolve naturally as you use it.
