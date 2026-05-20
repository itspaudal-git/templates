# Obsidian Integration

Connect your dotclaude setup to Obsidian for a unified "second brain".

## Files in This Folder

| File | Purpose |
|------|---------|
| `setup.md` | Complete integration instructions (start here) |
| `monthly-sync-checklist.md` | Monthly sync workflow checklist |
| `templates/` | Ready-to-use templates for Obsidian notes |

## Quick Start

1. **Read** [setup.md](setup.md) for detailed instructions
2. **Copy templates** from `templates/` to your Obsidian vault
3. **Follow** the [monthly-sync-checklist.md](monthly-sync-checklist.md) each month

## What Gets Synced?

- **Prompts** — From `.claude/prompts/` to Obsidian `Prompts/` folder
- **Memories** — From `/memories/repo/` to Obsidian `Memory/Repo/` folder
- **Connections** — Backlinks between related prompts and memories

## Key Files You'll Create

In your Obsidian vault:

```
Your Vault/
├── Index.md                    (copy from templates/obsidian-index.md)
├── Prompts/
│   ├── Backend/                (your backend prompts)
│   ├── Frontend/               (your frontend prompts)
│   ├── Testing/                (your testing prompts)
│   └── ...
├── Memory/
│   ├── Repo/                   (project-specific memories)
│   ├── Cross-Project/          (reusable across projects)
│   └── ...
└── _Attachments/               (optional: for images, diagrams)
```

## Integration Checklist

- [ ] Read [setup.md](setup.md)
- [ ] Add vault path to root `CLAUDE.md`
- [ ] Create folder structure in Obsidian
- [ ] Copy prompt templates to Obsidian
- [ ] Run initial sync: copy `.claude/prompts/` to Obsidian
- [ ] Set up monthly sync workflow
- [ ] Add backlinks and tags in Obsidian

## Next Steps

1. **Start with [setup.md](setup.md)** — Detailed configuration
2. **Use [templates/](templates/)** — Copy these into your Obsidian vault
3. **Run monthly sync** — Use [monthly-sync-checklist.md](monthly-sync-checklist.md)

## Questions?

- How do I keep prompts in sync? → See `setup.md` "Sync Workflow"
- What's the best way to organize? → See `setup.md` "File Templates for Obsidian"
- How often should I sync? → See `monthly-sync-checklist.md`
- How do I use Obsidian's graph view? → See `monthly-sync-checklist.md` "Using Obsidian to Discover Patterns"
