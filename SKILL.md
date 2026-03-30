---
name: component-to-figma
description: Extract React components from this codebase, render them on a clean temporary page optimized for Figma auto-layout, capture via Figma MCP, then clean up. Use when user says "send to Figma", "push to Figma", "capture components to Figma", "export to Figma", or wants to get components from the running app into a Figma file. Also triggers on "component to Figma", "widgets to Figma", or references sending UI elements to a Figma document.
---

# Component to Figma

Extract components from this Next.js codebase, render them on a temporary page optimized for Figma's auto-layout capture, send to Figma via `generate_figma_design`, then clean up.

## Workflow

### 1. Identify Components

Read the source file(s) the user references. Identify the React components to extract. Note:
- Component names, props, and static data
- Decorative transforms to strip (rotation, blur, scale, absolute positioning, animations)
- Render the default/resting state only (no hover/interactive states)

### 2. Create Temp Showcase Page

Create `src/app/_figma-capture/page.tsx`.

Rules for the temp page — see [references/auto-layout-patterns.md](references/auto-layout-patterns.md) for full examples:

- `"use client"` directive at top
- **Inline styles only** (`style={{}}` objects, not Tailwind). Figma capture maps inline styles to auto-layout more reliably.
- **No transforms** — zero rotation, blur, scale, absolute positioning
- **No animation** — no Framer Motion, no CSS transitions/keyframes
- **Static data** — default state, no interactivity or state hooks
- **Fixed widths** on each component card
- **Flexbox layout** — `display: "flex"`, `flexDirection`, `gap`, `alignItems` map directly to Figma auto-layout
- **White background** on page container, `padding: 64`, `gap: 32` between items
- Wrap each component in a card with a labeled header (see reference file for pattern)

### 3. Capture to Figma

Execute in this order:

1. Verify dev server: `lsof -i :5000 -sTCP:LISTEN`
2. Verify page: `curl -s -o /dev/null -w "%{http_code}" http://localhost:5000/_figma-capture` (expect 200)
3. Inject capture script in `src/app/layout.tsx` — add `<head><script src="https://mcp.figma.com/mcp/html-to-design/capture.js" async></script></head>` before `<body>`
4. Call `generate_figma_design` with `outputMode: "existingFile"`, target `fileKey` (and optionally `nodeId`)
5. Open the page:
   ```
   open "http://localhost:5000/_figma-capture#figmacapture={CAPTURE_ID}&figmaendpoint=https%3A%2F%2Fmcp.figma.com%2Fmcp%2Fcapture%2F{CAPTURE_ID}%2Fsubmit&figmadelay=2000"
   ```
6. Wait 8 seconds, then poll `generate_figma_design` with `captureId` until `completed`
7. Verify with `get_screenshot`

### 4. Clean Up

After successful capture:

1. Remove capture script from `src/app/layout.tsx` (restore original `<html>` without `<head>`)
2. Delete `src/app/_figma-capture/` directory: `rm -rf src/app/_figma-capture`
3. Confirm to user with the Figma link

Skip cleanup only if the user explicitly says to keep the temp page.

## Required MCP Tools

- `generate_figma_design` — capture ID + polling
- `get_screenshot` — verify result

## User Must Provide

- Which components/page to extract
- Target Figma file URL or file key
