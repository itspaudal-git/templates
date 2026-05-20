# Obsidian Integration

Connect your dotclaude prompts and memory to Obsidian for your "second brain".

## Setup

### 1. Vault is Already Set Up

Your Obsidian vault is in the project root:

```markdown
## Obsidian Vault

- **Vault Path**: `./obsidian/`
- **Prompts Folder**: `./obsidian/Prompts/`
- **Memory Folder**: `./obsidian/Memory/`
```

This is already configured in your root `CLAUDE.md`.

### 2. Folder Structure Already Ready

Your `./obsidian/` folder includes:

```
./obsidian/
├── Prompts/
│   ├── Backend/
│   ├── Frontend/
│   └── Testing/
├── Memory/
│   ├── Repo/
│   ├── Session/
│   └── Cross-Project/
├── Index.md  (start here)
├── Templates/
└── Other vault files
```

Just open `./obsidian/` as your Obsidian vault in the app.

### 3. Sync Prompts to Obsidian

#### Option A: Manual Sync (Simplest)

```bash
# Copy prompts to Obsidian
cp -r .claude/prompts/* ./obsidian/Prompts/

# Copy memories to Obsidian
cp /memories/repo/* ./obsidian/Memory/Repo/
```

#### Option B: Automatic Sync with Symlinks

```bash
# Link prompts folder so changes sync automatically
ln -s $(pwd)/.claude/prompts ./obsidian/Prompts/_source
```

Then edit either location and both stay in sync.

#### Option C: Git-based Sync

Since both `.claude/prompts/` and `./obsidian/` are in the same git repo:

1. Edit `.claude/prompts/` as your source of truth
2. Commit changes regularly
3. Copy to `./obsidian/` monthly during sync workflow
4. Both versions tracked in git history

---

## File Templates for Obsidian

When you export prompts to Obsidian, use this format:

### Prompt Note Template

```markdown
---
type: prompt
topic: "[Topic Name]"
skill: "/tdd"  # /refactor, /debug-fix, /ship, etc.
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
- [[Domain Knowledge]]

## Origin

From: `.claude/prompts/backend/api-validation.md`
```

### Memory Note Template

```markdown
---
type: memory
scope: "repo"  # user, session, or repo
tags: [database, performance]
created: 2026-05-19
updated: 2026-05-19
dotclaude-path: "/memories/repo/database.md"
---

# [Memory Title]

[Your memory notes here]

## Related

- [[Prompt: API Design]]
- [[Rule: Database Standards]]
```

---

## Sync Workflow

### When You Save a New Prompt

1. Save to `.claude/prompts/[domain]/name.md`
2. Tell Claude: "Add this to Obsidian under Prompts/[Domain]/"
3. Claude creates the Obsidian note with proper frontmatter
4. Reference it from your Obsidian Index

### When You Update Memory

1. Update `/memories/repo/` or `/memories/session/`
2. Export to Obsidian occasionally
3. Add backlinks between related notes

### Monthly Organization

Run this workflow monthly:

```bash
# 1. Check what changed in prompts/
git diff .claude/prompts/

# 2. Export new/updated prompts to Obsidian
# (manually copy or use a script)

# 3. In Obsidian, add backlinks and tags
# (Obsidian's graph view helps you see connections)

# 4. Archive old prompts
mkdir .claude/prompts/_archive/
mv .claude/prompts/[old]/name.md .claude/prompts/_archive/
```

---

## Opening Your Vault in Obsidian

1. Open Obsidian
2. Click "Open folder as vault"
3. Navigate to `./obsidian/` in your project
4. Click "Open"

Now you can browse and edit prompts and memories directly in Obsidian.

## Obsidian Plugins That Help

Recommended Obsidian plugins for prompt management:

- **Dataview** — Query your prompts by tag or type
- **Templater** — Auto-generate prompt notes from templates
- **Backlinks** — See connections between prompts and memories
- **Tag Wrangler** — Organize tags across prompts
- **Quick Capture** — Quickly save new prompts while browsing

### Example Dataview Query

Add to your Obsidian `Index.md` (or use the template):

```dataview
table skill, tags, updated
from "Prompts"
where type = "prompt"
sort updated descending
```

This shows all prompts sorted by when you last updated them.

---

## Integration Example

Your workflow might look like:

1. **Work in Claude** → Ask questions, iterate with Claude
2. **Save in dotclaude** → `.claude/prompts/backend/thing.md`
3. **Copy to Obsidian** → `cp .claude/prompts/backend/thing.md ./obsidian/Prompts/Backend/`
4. **Build graph** → Add tags and backlinks in Obsidian
5. **Reference** → Use Obsidian graph to find related prompts next time
6. **Update** → Edit `.claude/prompts/` (source), then copy to `./obsidian/` during monthly sync

Or use symlinks (Option B) for automatic sync in both directions.

---

## Troubleshooting

### Files not syncing

If using iCloud/Dropbox:
- Check that both folders are in the sync path
- Restart Obsidian
- Verify file permissions

### Backlinks broken

- Use relative paths in links: `[[../Backend/api-design|API Design]]`
- Keep folder structure consistent between dotclaude and Obsidian

### Too many files?

- Archive old prompts to `_archive/` monthly
- Use Obsidian's search to find what you need
- Rely on tags and graph view for organization

---

## Next Steps

1. ✅ Open `./obsidian/` as your Obsidian vault
2. ✅ Copy your first few prompts: `cp .claude/prompts/* ./obsidian/Prompts/`
3. ✅ Copy your memories: `cp /memories/repo/* ./obsidian/Memory/Repo/`
4. ✅ Add backlinks and tags in Obsidian
5. ✅ Use Obsidian's graph view to see connections
6. ✅ Run monthly sync using [monthly-sync-checklist.md](monthly-sync-checklist.md)
