# Getting Started with dotclaude

A complete step-by-step guide to set up and use dotclaude in your project.

## What is dotclaude?

dotclaude is a lean Claude Code configuration system that includes:
- **5 Specialist Agents**: Code reviewer, security reviewer, performance reviewer, doc reviewer, frontend designer
- **9 Workflow Skills**: Debug, ship, PR review, TDD, refactor, explain, test-writer, and more
- **6 Modular Rules**: Code quality, security, testing, error handling, frontend, database
- **Safety Hooks**: Protect sensitive files, auto-format, block dangerous commands, and more

It enhances your Claude Code workflow without opinions you can't override.

---

## Step 1: Initial Setup

### Option A: Automatic Setup (Recommended)

1. **Open Claude Code in your project**
   - Open your project in VS Code
   - Open Claude Code chat (Cmd+I)

2. **Run the setup skill**
   ```
   /setupdotclaude
   ```

   This will:
   - Detect your project type (language, framework, package manager, test runner, linter)
   - Copy dotclaude templates to `.claude/` in your project root
   - Auto-customize all config files to match your stack
   - Ask for confirmation before applying changes

3. **Verify the setup**
   ```bash
   ls -la .claude/
   ```
   You should see: `CLAUDE.md`, `settings.json`, `rules/`, `hooks/`, `skills/`, `agents/`

### Option B: Manual Setup

1. **Copy dotclaude to your project**
   ```bash
   cp -r /path/to/dotclaude .claude
   cd .claude
   ```

2. **Rename configuration templates**
   ```bash
   mv CLAUDE.local.md.example CLAUDE.local.md
   mv settings.local.json.example settings.local.json
   ```

3. **Copy root CLAUDE.md**
   ```bash
   cp CLAUDE.md ../CLAUDE.md
   ```

4. **Customize for your project** (see Step 3 below)

---

## Step 2: Enable Hooks (Safety & Automation)

Hooks are shell scripts that run on git events and protect your workflow.

1. **Make hooks executable**
   ```bash
   chmod +x .claude/hooks/*.sh
   ```

2. **Verify jq is installed** (required for hooks)
   ```bash
   which jq
   ```
   
   If not installed:
   ```bash
   # macOS
   brew install jq
   
   # Linux
   apt install jq
   ```

3. **Test a hook**
   ```bash
   ./.claude/hooks/format-on-save.sh
   ```

### What Each Hook Does

| Hook | Purpose |
|------|---------|
| `format-on-save.sh` | Auto-formats staged changes using project formatter |
| `protect-files.sh` | Blocks commits that modify protected files (`.env`, `keys/`) |
| `block-dangerous-commands.sh` | Prevents accidental `git push -f` to main branch |
| `scan-secrets.sh` | Detects leaked secrets before commit |
| `auto-test.sh` | Runs tests automatically on commit |
| `context-recovery.sh` | Recovers conversation history after crashes |

---

## Step 3: Customize Rules & Skills

### Add Project-Specific Rules

Rules define coding standards, patterns, and constraints for Claude.

1. **Create a new rule**
   ```bash
   touch .claude/rules/my-rule.md
   ```

2. **Example: Payment Processing Rule**
   ```yaml
   ---
   paths:
     - "src/billing/**"
     - "src/payments/**"
   ---

   # Payment Processing Standards

   - All monetary amounts use cents (integers), never floats
   - Use the `tax-engine` service for all tax calculations
   - Every payment mutation requires a unique `idempotency_key`
   - PCI-DSS: Never log card numbers or tokens
   ```

   The `paths:` frontmatter scopes this rule to specific files only.

3. **Restart Claude Code** (rules load at session start)
   - Close Claude Code
   - Reopen the chat (Cmd+I)

### Add Custom Skills

Skills are slash commands that help you with specific workflows.

1. **Create a skill directory**
   ```bash
   mkdir -p .claude/skills/my-skill
   ```

2. **Create SKILL.md**
   ```bash
   cat > .claude/skills/my-skill/SKILL.md << 'EOF'
   ---
   description: My custom workflow description
   ---

   # My Custom Skill

   Use `/my-skill` to trigger this workflow.

   ## Steps

   1. User runs `/my-skill [description]`
   2. Claude does X
   3. Claude asks for confirmation
   4. Implementation happens

   ## Best Practices

   - Always confirm before making changes
   - Provide clear feedback at each step
   EOF
   ```

3. **Use your skill**
   ```
   /my-skill [your input]
   ```

---

## Step 4: Using Claude Memory (Best Practices)

Claude memory helps you and Claude learn from past sessions and build on previous work.

### Understanding Memory Scopes

There are **three types of memory** in dotclaude:

| Scope | Location | Lifetime | Use Case |
|-------|----------|----------|----------|
| **User** | `/memories/` | Permanent across all projects | Preferences, patterns, lessons learned |
| **Session** | `/memories/session/` | Current conversation only | Task-specific context, plans, progress |
| **Repo** | `/memories/repo/` | This project only | Project facts, conventions, build commands |

### Update Memory Manually

In Claude Code chat, you can update memory by telling Claude:

```
Remember: TypeScript strict mode is enabled in this project. Always use type annotations.
```

Claude will automatically save this to `/memories/repo/notes.md`.

### Update Memory with a File

1. **Tell Claude what to remember**
   ```
   Create a memory note: In this project, we use pnpm for package management, always run pnpm commands not npm. Store this in /memories/repo/dependencies.md
   ```

2. **Claude creates/updates the file**
   - Claude will read existing memory
   - Update or create the file with your note
   - You can review and confirm changes

### Memory File Templates

**User Memory** (shared across all projects)
```markdown
# Development Preferences
- Prefer TypeScript for type safety
- Always write tests for new features
- Use relative imports in ES modules

# Common Fixes
- Jest test watch mode sometimes needs --no-coverage flag
- Docker volume mounts on macOS need full paths
```

**Repo Memory** (project-specific)
```markdown
# Build System
- Uses pnpm, not npm
- Build command: `pnpm run build`
- Tests run with: `pnpm test`

# Database
- PostgreSQL 15+
- Migrations in `db/migrations/`
- Use knex for schema

# Deployment
- Staging: Github Actions on PR
- Prod: Manual approval via Slack
```

### Best Practices for Memory

✅ **DO:**
- Keep memory concise (bullet points, short sentences)
- Record "why" decisions, not obvious facts
- Update when you discover something new
- Use separate files for different topics (build.md, database.md, security.md)

❌ **DON'T:**
- Write lengthy prose (memory is loaded into every conversation)
- Record things already in code comments or docs
- Duplicate information across memory files
- Store credentials or secrets

### Check Memory Usage

1. **Run the context budget check**
   ```
   /context-budget
   ```

   This shows how much token budget your `.claude/` and memory files use.

2. **If memory is too large**
   - Consolidate similar notes
   - Archive old notes (move to a `_archive/` folder)
   - Remove stale information

---

## Step 5: Use Specialist Agents

Agents are Claude instances with specialized knowledge.

### Auto-Delegated Agents

Agents are automatically assigned based on your task:

- `@code-reviewer` — Auto-runs on `/pr-review` code checks
- `@security-reviewer` — Auto-runs if you touch auth, encryption, or secrets
- `@performance-reviewer` — Auto-runs if you change database queries or rendering code
- `@doc-reviewer` — Auto-runs if you update documentation

### Invoke Agents Directly

You can explicitly request an agent:

```
@security-reviewer Review the auth middleware in src/auth/middleware.ts for vulnerabilities
```

```
@code-reviewer Check my staged changes before shipping
```

```
@frontend-designer Create a dark-mode analytics dashboard
```

Agents run with isolated context but can read your full codebase.

---

## Step 6: Use Workflow Skills

### Essential Skills

**`/setupdotclaude`** — Bootstrap dotclaude in a new project
```
/setupdotclaude
```

**`/debug-fix`** — Find and fix bugs systematically
```
/debug-fix "Navigation dropdown not closing" --fast
```
(Use `--fast` for production hotfixes)

**`/tdd`** — Strict red-green-refactor TDD loop
```
/tdd "Add rate limiting to API endpoints"
```

**`/refactor`** — Safe refactoring with tests
```
/refactor src/api/handlers.ts
```

**`/explain`** — Understand code deeply
```
/explain src/database/migrations
```

**`/ship`** — Complete shipping workflow
```
/ship "Add user authentication"
```
Stages files, commits, pushes, and creates a PR.

**`/pr-review`** — Comprehensive PR reviews
```
/pr-review 42
```
Delegates to code, security, performance, and doc reviewers.

### Auto-Triggering Skills

**`/test-writer`** — Automatically writes tests for new code
- Runs after you add new features
- Discovers code paths: happy, edge cases, errors
- Writes one test per scenario with Arrange-Act-Assert

---

## Step 7: Daily Workflow Examples

### Example 1: Add a Feature with TDD

```
1. Open Claude Code (Cmd+I)
2. /tdd "Add email verification on signup"
3. Claude writes a failing test
4. You implement the minimum code to pass
5. Claude refactors and commits
6. Repeat until feature is complete
7. /ship "Add email verification"
```

### Example 2: Fix a Bug Safely

```
1. /debug-fix "Users can't reset password"
2. Claude reproduces the issue
3. Claude writes a regression test
4. Claude fixes the bug
5. Tests pass, code is committed
```

### Example 3: Review a PR Before Merge

```
1. /pr-review 42
2. Agents check code, security, performance, docs
3. Claude synthesizes findings with severity levels
4. Review findings in your editor
5. Address feedback, request re-review
```

### Example 4: Add Project Memory

```
1. Discover: "We always use pnpm, not npm"
2. Tell Claude: "Remember: This project uses pnpm. Store in /memories/repo/package-manager.md"
3. Claude saves the note
4. In future sessions, Claude automatically knows pnpm is the standard
```

---

## Step 8: Integrate with Obsidian (Your Second Brain)

If you use Obsidian, sync your prompts and memories to your vault for cross-project reference.

### Quick Setup

1. **Vault is already set up** at `./obsidian/` in your project root
   - Check root `CLAUDE.md` for vault path
   - Prompts sync to `./obsidian/Prompts/`
   - Memories sync to `./obsidian/Memory/`

2. **Folders already exist in `./obsidian/`**
   ```
   ./obsidian/
   ├── Prompts/
   ├── Memory/
   └── Index.md
   ```

3. **Sync your prompts to `./obsidian/Prompts/`**
   ```bash
   cp -r .claude/prompts/* ./obsidian/Prompts/
   ```
   - Manual: Copy `.claude/prompts/` files to `./obsidian/Prompts/`
   - Ask Claude: "Export this to ./obsidian/Prompts/[domain]/"
   - Git: Both `.claude/prompts/` and `./obsidian/` are tracked together

4. **Build your graph**
   - Add tags to organize prompts
   - Use backlinks to connect related prompts and memories
   - Use Obsidian graph view to see connections

See [dotclaude/obsidian/setup.md](obsidian/setup.md) for detailed integration instructions.

---

## Step 9: Build Your Prompt Library

Over time, you'll discover prompts that work really well. Save them so you can reuse and refine them.

### Why Save Prompts?

- Reuse successful approaches across projects
- Build a personal reference library
- Document the evolution from "broken English" to refined prompts
- Share best practices with team members

### How to Organize Prompts

1. **Create your first prompt**
   ```bash
   mkdir -p .claude/prompts/[domain]
   touch .claude/prompts/[domain]/prompt-name.md
   ```

2. **Choose an organization strategy**
   - **By Domain** (backend, frontend, testing, debugging)
   - **By Skill Type** (/tdd, /refactor, /ship, /debug-fix)
   - **By Project** (if managing multiple projects)

   See [prompts/README.md](prompts/README.md) for detailed strategies.

3. **Save prompts with context**
   ```markdown
   ---
   topic: "API Design"
   tags: ["backend", "validation"]
   ---

   # My API Design Prompt

   ## Original (Broken English)
   > how do i design api that validates all input really good

   ## Refined Prompt
   [Your improved version]

   ## When to Use
   [When this works best]
   ```

### Example: Save Your First Prompt

After a successful conversation:

```
Create a file .claude/prompts/backend/api-validation.md with:
- My original broken phrasing
- The refined version that worked
- When to use it
```

Claude can organize and file this for you automatically.

### Organize as You Grow

Your prompt library will grow naturally. Each month:
- Move rarely-used prompts to `_archive/`
- Consolidate similar variations
- Update tags for easier discovery
- Share with team in `.claude/` git commits

---

## Step 10: Troubleshooting

### Skills or agents don't appear

**Fix:** Restart Claude Code
- Close the chat window
- Open a new chat (Cmd+I)
- Everything reloads at session start

### Hooks not executing

**Fix:** Check permissions and jq installation
```bash
chmod +x .claude/hooks/*.sh
which jq  # must be installed
```

### "jq not found" errors

**Fix:** Install jq
```bash
# macOS
brew install jq

# Linux
apt install jq
```

### format-on-save not working

**Fix:** Verify formatter is installed
```bash
# For prettier
npm list prettier

# For ESLint
npm list eslint

# And config files exist
ls -la .prettierrc  # or .eslintrc.json
```

### Protected files still getting committed

**Fix:** Check `settings.json` protect-files section
```json
{
  "protect-files": {
    "paths": [".env", ".env.local", "secrets/"]
  }
}
```

### Claude doesn't remember previous sessions

**Fix:** Check memory file permissions
```bash
ls -la /memories/
ls -la /memories/repo/
```

Files should be readable/writable by your user.

---

## Step 11: Best Practices Summary

### Project Organization

✅ Keep `.claude/` in your project root  
✅ Keep root `CLAUDE.md` lean (under 25 lines after setup)  
✅ One rule file per domain (billing.md, auth.md, database.md)  
✅ Scope rules with `paths:` frontmatter to specific directories  

### Memory Management

✅ Use `/memories/repo/` for project facts  
✅ Use `/memories/session/` for current task context  
✅ Use `/memories/` for cross-project patterns  
✅ Keep all memory entries concise (2-3 sentence max)  
✅ Run `/context-budget` monthly to monitor token usage  

### Git Integration

✅ Enable all hooks after setup  
✅ Use `/ship` for complex commits  
✅ Use `/debug-fix --fast` only for production emergencies  
✅ Run `/pr-review` before merging  

### Collaboration

✅ Share `.claude/` folder with team (git commit it)  
✅ Use `CLAUDE.local.md` for personal overrides only  
✅ Use `settings.local.json` for local-only settings  
✅ Document team standards in `.claude/rules/`  

---

## Quick Reference

| Want to... | Do this |
|-----------|---------|
| Add a coding rule | Create `.claude/rules/my-rule.md` |
| Create a workflow | Create `.claude/skills/my-skill/SKILL.md` |
| Add a specialist | Create `.claude/agents/my-agent.md` |
| Remember something | Tell Claude: "Remember: [fact]" or edit `/memories/` directly |
| Save a working prompt | Create `.claude/prompts/[domain]/name.md` with context |
| Reuse a saved prompt | Copy from `.claude/prompts/` and adapt for new task |
| Check what Claude sees | Run `/context-budget` |
| Review code safely | Use `/pr-review [PR#]` |
| Write tests first | Use `/tdd [feature description]` |
| Refactor safely | Use `/refactor [file or function]` |
| Fix a bug systematically | Use `/debug-fix [issue description]` |
| Ship code confidently | Use `/ship [commit message]` |

---

## Step 12: Next Steps

1. ✅ Run `/setupdotclaude` to customize for your project
2. ✅ Enable hooks with `chmod +x .claude/hooks/*.sh`
3. ✅ Review `.claude/rules/` and add project-specific rules
4. ✅ Start using `/tdd`, `/debug-fix`, or `/ship` on your next task
5. ✅ Document your first memory with `/memories/repo/`
6. ✅ Save your first working prompt to `.claude/prompts/`
7. ✅ Integrate with Obsidian using `.claude/obsidian/setup.md`

Happy coding! 🚀
