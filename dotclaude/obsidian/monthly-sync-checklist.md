# Obsidian Sync Checklist

Use this checklist to sync prompts and memories to your Obsidian vault.

## Initial Setup (One-time)

- [ ] Create `Prompts/` folder in Obsidian vault
- [ ] Create `Memory/` folder in Obsidian vault
- [ ] Create subfolders: `Backend/`, `Frontend/`, `Testing/`, etc.
- [ ] Add vault path to root `CLAUDE.md`
- [ ] Copy initial prompts from `.claude/prompts/` to Obsidian
- [ ] Copy initial memories from `/memories/repo/` to Obsidian
- [ ] Add tags to prompts (use Obsidian's tag system)
- [ ] Create backlinks between related prompts

## Monthly Sync Workflow

### Week 1: Review & Archive

```bash
# Check which prompts you actually use
grep -l . .claude/prompts/**/*.md | sort

# Archive prompts you haven't touched in 3+ months
mkdir .claude/prompts/_archive
mv .claude/prompts/[old]/unused.md .claude/prompts/_archive/
```

- [ ] Archive unused prompts in `.claude/prompts/_archive/`
- [ ] Delete archived prompts from Obsidian
- [ ] Review memory files for stale information

### Week 2: Update & Sync

```bash
# Copy new/updated prompts to ./obsidian/
cp .claude/prompts/*/*.md ./obsidian/Prompts/

# Copy updated memories to ./obsidian/
cp /memories/repo/*.md ./obsidian/Memory/Repo/
```

- [ ] Copy new prompts to Obsidian
- [ ] Copy updated memories to Obsidian
- [ ] Add frontmatter with `dotclaude-path` to new notes
- [ ] Tag new prompts

### Week 3: Connect & Build Graph

- [ ] Add backlinks between related prompts (`[[Other Prompt]]`)
- [ ] Add tags to organize by domain/skill
- [ ] Review Obsidian graph view for connections
- [ ] Update Obsidian Index.md with latest changes

### Week 4: Document & Plan

- [ ] Note any prompt patterns you discovered
- [ ] Plan new prompts for next month
- [ ] Update this checklist if workflow changed

## When Saving a New Prompt

1. [ ] Save to `.claude/prompts/[domain]/name.md`
2. [ ] Tell Claude: "Add this to Obsidian under Prompts/[Domain]/"
3. [ ] Claude creates Obsidian note with frontmatter
4. [ ] Add backlinks in Obsidian to related prompts
5. [ ] Add tags

## When Updating Existing Prompt

1. [ ] Update `.claude/prompts/[domain]/name.md`
2. [ ] Update corresponding Obsidian note
3. [ ] Update `updated:` date in frontmatter
4. [ ] Optional: Update related backlinks if scope changed

## Obsidian Maintenance

### Check Sync Status

```bash
# Files in dotclaude but not in Obsidian
diff -r .claude/prompts ~/Obsidian/your-vault/Prompts/

# Files in Obsidian but not in dotclaude (should be minimal)
diff -r ~/Obsidian/your-vault/Prompts/ .claude/prompts/
```

### Troubleshoot Broken Links

1. Open Obsidian graph view
2. Look for broken links (red edges)
3. Fix references or delete stale notes

### Query All Prompts

Add to Obsidian Index.md:

```dataview
table topic, skill, tags, updated
from "Prompts"
where type = "prompt"
sort updated descending
```

## Using Obsidian to Discover Patterns

1. **Graph View**: See which prompts are most referenced
2. **Tags**: Group prompts by domain/skill/project
3. **Backlinks**: Find prompts that solve similar problems
4. **Search**: `tag:#api` to find all API-related prompts

## Template for New Prompt Files

```markdown
---
type: prompt
topic: "[Topic Name]"
skill: "/tdd"  # or /refactor, /debug-fix, /ship, /explain
tags: [backend, api, validation]
created: 2026-05-19
updated: 2026-05-19
dotclaude-path: ".claude/prompts/backend/api-validation.md"
---

# [Prompt Title]

## When to Use

[Describe when this prompt works best]

## The Prompt

[Your refined prompt text here]

## Example Output

[What Claude produces with this prompt]

## Related Notes

- [[Other Prompt]]
- [[Memory Note]]
```

## Tips

- Keep `.claude/prompts/` as source of truth
- Obsidian is for discovery and organization
- Monthly sync prevents drift between systems
- Use backlinks to see patterns
- Archive old prompts to keep Obsidian lean
