---
name: multiagent-conventions
description: Conventions for working inside the MultiAgent desktop app's terminal (Claude/Codex sessions launched by the MultiAgent app). Use when referencing, citing, or printing file/folder paths so the terminal turns them into clickable links, and to follow the app's terminal UX conventions. Apply whenever the current session runs inside MultiAgent.
---

# MultiAgent terminal conventions

MultiAgent is a Tauri desktop app that runs Claude/Codex sessions in xterm-based
terminals. The terminal turns recognized file/folder paths in your output into
**clickable links** (open a file in the Docs panel, open an image in the in-app
viewer, or jump to a specific line). To make this work, follow the rules below
whenever you mention or cite files.

## File path formatting (most important)

Always reference files with a **path that includes at least one directory
segment**, or an **absolute path**. Never print a bare filename on its own.

- ✅ Good: `app/src/components/PaneSlot.tsx`, `docs/README.md`, `K:\AI\MultiAgent\app\package.json`
- ❌ Bad (not linked): `PaneSlot.tsx`, `package.json`

Why: the terminal's link detector requires a directory segment to recognize a
code/general file path. A lone filename is treated as plain text.

**Do not prefix the path with the current project's own folder name.** Relative
paths are resolved against the project root (the cwd), so adding the folder name
doubles it and resolution fails.

- (cwd is `...\Project5800`) ❌ `Project5800/Docs/UDS_Reverse/00_Weather_System_Roadmap.html`
- ✅ `Docs/UDS_Reverse/00_Weather_System_Roadmap.html`  (or an absolute path)
- When unsure, use an **absolute path** — it always resolves.

### Specifics

- **Code / general files** (`.ts`, `.rs`, `.py`, `.json`, …): need **≥1 directory
  segment**. `./foo.ts` and `../foo.ts` are **not** recognized. For a file that
  sits directly at the project root with no subfolder, use an **absolute path**.
- **Markdown / HTML / images** (`.md`, `.html`, `.png`, `.jpg`, …): a bare
  filename *is* recognized, but include the path anyway for consistency.
- **Jump to a line**: append `:line` (or `:line:col`), e.g.
  `app/src/lib/terminal.ts:72`. Clicking opens that file at the line.
- Use forward slashes for relative paths. Absolute Windows paths (`K:\...`) and
  UNC paths (`\\host\share\...`) also work.

## Terminal UX you can rely on

- Clicking a `.md` / `.html` path opens it in the **Docs panel** (works even for
  paths outside the project folder).
- Clicking an image path (`.png/.jpg/.gif/.webp/.bmp/.svg/.ico`) opens the
  **in-app image viewer**.
- Practical implication: whenever you produce a report, a screenshot, or a
  generated file, print its path in linkable form so the user can open it in one
  click.
