# profile-creator

A Claude Code plugin that builds JG-branded candidate profiles — a print/PDF-ready one-pager presenting a candidate to a client for a specific role.

## What's in it

| Skill | What it does |
|---|---|
| `jg-candidate-profile-creator` | Builds the branded one-page profile from a resume + target role |

The skill ships the full template bundle in `skills/jg-candidate-profile-creator/templates/` — layout, design-system CSS, brand logos, and the runtime needed to render and export it. Nothing is fetched from a design project at build time.

## Install

Local, for one project — add to `.claude/settings.json`:

```json
{ "extraKnownMarketplaces": {}, "enabledPlugins": [] }
```

Or point Claude Code at the directory directly. For team distribution, publish this
directory in a marketplace repo (a git repo with `.claude-plugin/marketplace.json`
listing it) and install with `/plugin install profile-creator@<marketplace>`.

## Origin

Ported from the Claude Design project `695554cf-4e21-4c17-852f-aabe5ea30afe`
("Candidate Profile Clean"). Two things changed in the port, because the design
canvas provided them implicitly and a plugin cannot:

1. **`support.js` and the `_ds/` design-system CSS now ship inside `templates/`.**
   In the canvas they lived at the project root and were bound automatically.
2. **`doc-page.js` ships in `templates/` too**, replacing the canvas-only
   `copy_starter_component({kind: "doc_page.js"})` call the original SKILL.md
   referenced, and the pagination convention that lived in the design project's
   `CLAUDE.md` is folded into SKILL.md directly.

## Rendering note

The templates use the design-canvas runtime (`x-dc` / `dc-import` / `DCLogic`).
`Candidate Profile.dc.html` fetches `ProfileVariant.dc.html` as a sibling at
runtime, so the directory must be served over **HTTP** — `file://` will fail on
same-origin rules. `support.js` loads React, ReactDOM, and Babel from unpkg at
runtime, so first render needs network access.
