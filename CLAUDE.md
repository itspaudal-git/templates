# Project Instructions

> This root `CLAUDE.md` is the main project-wide entrypoint for Claude Code. `./dotclaude/` remains the plugin source folder and can be referenced for additional implementation details.

## Obsidian Vault

Your Obsidian vault is in the project root at `./obsidian/`.

- **Vault Path**: `./obsidian/`
- **Prompts Folder**: `./obsidian/Prompts/`
- **Memory Folder**: `./obsidian/Memory/`

See `./dotclaude/obsidian/setup.md` for sync instructions and templates.

## Commands

```bash
# Build
pnpm run build            # or: cargo build, go build ./..., make build

# Test
pnpm test                 # run full suite
pnpm test -- path/to/file # run single test file

# Lint & Format
pnpm run lint             # check style
pnpm run lint:fix         # auto-fix style
pnpm run typecheck        # type checking

# Dev
pnpm run dev              # start dev server
```

## Architecture

> REPLACE: Describe non-obvious architectural decisions. Don't list files; Claude can explore.

- `src/`. Application source.
- `src/api/`. REST endpoints (versioned: `/v1/`).
- `src/services/`. Business logic (no direct DB access from controllers).
- `src/models/`. Data models and types.

## Key Decisions

> REPLACE: Record WHY non-obvious choices were made. This is the most valuable section. Examples: "Auth tokens in httpOnly cookies because XSS risk", "Billing is a separate module for audit independence".

## Domain Knowledge

> REPLACE: Terms, abbreviations, or concepts that aren't obvious from the code. Example: "SKU" = Stock Keeping Unit, the unique product identifier from our warehouse system.

## Workflow

- Run typecheck after making a series of code changes
- Prefer fixing the root cause over adding workarounds
- When unsure about approach, use plan mode (`Shift+Tab`) before coding

## Don'ts

- Don't modify generated files (`*.gen.ts`, `*.generated.*`)