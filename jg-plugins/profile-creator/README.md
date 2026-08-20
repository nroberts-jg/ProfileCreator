# profile-creator

A Claude Code plugin that builds JG-branded candidate profiles — a print/PDF-ready one-pager presenting a candidate to a client for a specific role.

## What's in it

| Skill | What it does |
|---|---|
| `jg-candidate-profile-creator` | Builds the branded one-page profile from a resume + target role |

The skill ships the full template bundle in `skills/jg-candidate-profile-creator/templates/` — layout, design-system CSS, brand logos, and the runtime needed to render and export it. Nothing is fetched from a design project at build time.

## Install

This plugin is served by the `jg-plugins` marketplace in the parent directory.
Register the marketplace once, then install:

```
/plugin marketplace add /Users/nroberts/Claude/Projects/jg-plugins
/plugin install profile-creator@jg-plugins
```

To distribute to the team, push `Projects/jg-plugins/` to a git repo and have
teammates `add` the repo URL instead of the local path. Additional plugins drop
into that directory as sibling folders and get a new entry in
`.claude-plugin/marketplace.json`.

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
