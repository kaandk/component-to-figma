# /to-figma — Claude Code Skill

A Claude Code slash command that extracts React components from a Next.js codebase, renders them on a clean temporary page optimized for Figma's auto-layout system, captures them via the Figma MCP, and cleans up afterwards.

## What it does

1. **Identifies** the components you reference in your codebase
2. **Creates a temp page** (`_figma-capture`) with inline styles, no transforms/animations, static data — optimized for Figma auto-layout capture
3. **Captures to Figma** via `generate_figma_design` MCP tool
4. **Cleans up** the temp page automatically (unless you say to keep it)

## Install

Copy the `SKILL.md` and `references/` folder into your project's `.claude/skills/component-to-figma/` directory.

## Usage

Use the slash command in Claude Code:

```
/to-figma [screenshot or description of components] send to https://figma.com/design/...
```

Examples:
- `/to-figma [screenshot] send the widget cards to https://figma.com/design/abc123/MyFile`
- `/to-figma the showcase widgets on /creator page to https://figma.com/design/abc123/MyFile`

## Requirements

- Next.js project with a running dev server
- Figma MCP server connected to Claude Code
- A target Figma file URL or file key

## Files

```
component-to-figma/
├── SKILL.md                        # Skill instructions
└── references/
    └── auto-layout-patterns.md     # Figma-optimized inline style patterns
```
