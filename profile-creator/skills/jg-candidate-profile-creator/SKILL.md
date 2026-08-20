---
name: jg-candidate-profile-creator
description: Build a JG-branded one-page candidate profile (HTML, print/PDF-ready) that presents a candidate to a client for a specific role. Use whenever someone wants to present, pitch, submit, or package a candidate for a client — triggers on "build a candidate profile for [name]", "profile this person for [client]", "one-pager for [candidate]", "package [candidate] for the [role] opening", or when a resume is shared alongside a client and role. Produces the branded one-page profile for screen and PDF. Use THIS skill when a one-page profile, PDF, or shareable page is wanted; ask if the format is ambiguous.
---

# JG Candidate Profile Creator

Produces a Jahnel-Group-branded candidate profile (HTML, print/PDF-ready) sized to US Letter (816×1056px), built from a reusable template. Single page for most candidates; profiles with enough real project history to overflow one page paginate onto continuation pages rather than getting clipped or padded down. The profile gives a client resume-level depth — real project history with specific achievements, full categorized skills, industry experience — in a layout that stays scannable.

## What you need before building

Ask only for what's missing; infer the rest from the materials provided.

1. **Candidate resume or notes** (required) — the source of all content.
2. **Target role** (required) — drives skill category ordering and the "Submitted for" footer line.
3. **Client name** (optional) — appears in the footer alongside the role.
4. **Headshot** (optional) — the template has a drop-target; if none is provided, tell the user to drag one in, or turn the headshot off.

Never invent employers, metrics, dates, or credentials. If a fact isn't in the source, leave it out or mark it `TBD`.

**Anonymize client names.** Never print a real client/employer name in the experience entries — replace it with a generic descriptor of the industry or engagement type instead (e.g. "Healthcare Payments Platform," "Retail Fulfillment Client," "Enterprise Financial Services Firm"). This applies to the `entries` titles/industry fields and the intro's anchor-engagement mention. The target client name in the footer ("Submitted for") is the only real client name allowed on the page.

## How to build

1. Copy the **entire contents** of this skill's `templates/` directory into the working directory — it is self-contained and carries everything the page needs:

   | File | Role |
   |---|---|
   | `Candidate Profile.dc.html` | Entry file — the one you open and show the user |
   | `ProfileVariant.dc.html` | The layout + all candidate data (the only file you edit) |
   | `support.js` | Design-canvas runtime (loads React from CDN at runtime) |
   | `doc-page.js` | Print/pagination component — owns the page box and `@page` geometry |
   | `image-slot.js` | The headshot drop-target |
   | `_ds/jahnel-group-design-system-…/colors_and_type.css` | Anton + Libre Franklin, JG brand tokens |
   | `assets/jg-logo-light.svg`, `assets/jg-logo-dark.svg` | Header logo, light and dark variants |

   Rename the entry file to `Candidate Profile — [Name].dc.html`. Keep every other filename and the `_ds/` and `assets/` folder structure exactly as-is — the templates reference them by relative path.

2. Open `ProfileVariant.dc.html` and replace the candidate data in `renderVals()` — `name`, `role`, `intro`, `industries`, `entries`, `skills`, `education`, `school`, `funFact`, `monogram`. **This is the only file whose content you edit.**
3. Order the `skills` categories so the most role-relevant category is first (see Skills rules).
4. Verify the page still fits one letter page (see Fitting one page).
5. Serve the directory over HTTP and open `Candidate Profile — [Name].dc.html` to show the user. It must be HTTP, not `file://` — the runtime fetches `ProfileVariant.dc.html` as a sibling, which same-origin rules block on `file://`. Any static server works, e.g. `python3 -m http.server`.
6. Export to PDF via the browser's print dialog when they ask.

## Content rules

Full field-by-field spec in `reference/content-requirements.md`. The rules that matter most:

**Experience entries** — 3 projects, reverse-chronological. Each gets a title, date range, role + client industry, 2–4 achievement bullets, and a tech-stack line. Bullets are action-led and specific: what they owned, what they built, what it protected or enabled. Consolidate related facts into one strong bullet rather than padding with weak ones. No fabricated numbers.

**Skills** — every relevant skill listed, grouped into categories (typically Languages & Frameworks; Cloud, Platforms & Tools; Security & Compliance). **Never rate, rank, or emphasize skills** — no proficiency bars, dots, star ratings, or "core vs. also-knows" splits. The only thing that varies by role is **category order**: lead with the category the target role hires for (front-end skills first for a front-end role, cloud first for a DevOps role).

**Intro** — 3–4 sentences: tenure and anchor engagement, technical blend, then a trust note grounded in real client feedback. Left-aligned, not justified.

**Voice** — plain, direct, confident. Builders talking to builders. No marketing fluff, no emoji.

## Visual rules

Locked decisions — don't relitigate them per candidate:

- **Accent** `#39A7EB` — used for section headings, the hero divider fade, the headshot edge, and the availability capsule. Nothing else. Resist adding more blue.
- **Type scale** is exactly six steps: 38 (name) / 19 (role) / 15 (section headings) / 12.5 (body, bullets) / 11 (secondary labels, dates) / 10 (micro: stack lines, footer). Never introduce an in-between size.
- **Headings** are bold Libre Franklin, not Anton — Anton is reserved for the candidate name.
- **Fun Fact** sits in a tinted panel with an accent left bar; it's meant to pop.
- **Layout** is a two-column grid: experience left, a filled inlay rail right holding Industry Experience, Skills, Education, Fun Fact.
- **Footer** carries the confidentiality line and "Submitted for [role]".

## Tweaks available

The profile exposes these without code changes:

| Tweak | Options | Use |
|---|---|---|
| `theme` | dark / light | Light is print-safe |
| `skillStyle` | tags / list | Outlined tags or typographic list |
| `tagShape` | square / pill | Tag corner style — pill is the default |
| `showHeadshot` | on / off | Hide when no photo is available |
| `headshotShape` | hexagon / circle / rounded / square / diamond | Rounded is the default; hexagon is the brand shape |
| `showAvailability` + `availability` | toggle + text | "Available Now" or a specific date |
| `showRate` + `rate` | toggle + text | Off by default — only for internal or rate-transparent clients |
| `showStack` | on / off | Hides the per-project tech-stack lines |
| `submittedFor` | text | Footer role line |

## Print / PDF

Profiles are already print-owning — do not re-format, wrap, or rebuild them for export. `doc-page.js` owns the print geometry: it injects the `@page` rule (letter, zero margin) and the print-color overrides, and declares `meta[name="omelette-owns-print"]` so nothing layers page CSS on top. The page is authored at exactly 816×1056 (8.5×11 at 96dpi), so PDF export is 1:1 with the screen.

Do **not** write your own `@page` rules, page-break CSS, desk backgrounds, or fake page-card sheets alongside `doc-page` — it owns all of that. Do not set width or height on a `.page` section.

## Fitting one page vs. paginating

Try to fit one page first, in this order:
1. Consolidate bullets in the **oldest** entry down to 2.
2. Shorten the intro by a sentence.
3. Trim entry padding (currently 9px) and hero padding.
4. Turn `showStack` off.

Never shrink body type below 12.5px or bullets below 11px — these get printed and forwarded.

If the candidate genuinely has more real project history than one page holds even after those steps, **let it paginate rather than cutting real content**. The template already does this for you: `renderVals()` measures each entry, fills page 1, and flows the overflow onto continuation pages with a slim header and a "Page X of Y" footer. The sidebar (Industry Experience, Skills, Education, Fun Fact) stays on page 1 only. Don't silently clip content to force one page, and don't rebuild the layout to paginate — it is already built on `doc-page` explicit pagination, one `<section class="page">` per page.

Design every page to **fill** the letter page box and to fit letter and A4 alike — no viewport units, no content that overflows the box (overflow is clipped, not reflowed).

## Multiple candidates

One profile per file, named `Candidate Profile — [Name].dc.html`, each importing the shared `ProfileVariant.dc.html` with its own data. Don't fork the layout per candidate.

Note the headshot sidecar: dropped images persist to `.image-slots.state.json` next to the HTML, and pages in the same directory share one sidecar. Keep slot ids distinct across profiles — the template derives its id from the preset, so give each candidate their own directory when in doubt.
