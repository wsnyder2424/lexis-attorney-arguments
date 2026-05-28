# Lexis Attorney Arguments — Agent rules

This file is read by Claude Code (and any AI agent operating on this repo) at the start of every session. Treat it as binding context. Edit it as the prototype evolves; outdated rules will produce worse changes than no rules.

---

## What this is

A coded prototype of LexisNexis Context's Attorney Analytics module. Three static HTML pages deployed to GitHub Pages, intended for a live 30-minute case-study walkthrough. Wiring + content are specced in `prd-attorney-research-flow.md`. Design system tokens live in `styles.css` and `.interface-design/system.md`.

## Demo scenario

The viewer is told: "You're a plaintiff's-side IP attorney. You're going up against **Catherine Finnerty** in court next month. You want to see what she's argued in similar cases." The flow walks Overview → Arguments → Document Reader.

## Attorney profile

- Name: **Catherine Finnerty** — never Kathleen.
- Firm: Reed & Marston LLP. San Diego, CA.
- Bar admissions: California (2008), Ninth Circuit. 18 years practicing.
- Practice mix: IP-dominant (34%) per PRD § 6.

## Pages

- `overview.html` — Catherine's profile / Fact Sheet / litigation distribution / experience.
- `index.html` — Arguments page. Lands in the filtered-to-6 state (Practice Area: IP, Jurisdiction: S.D. Cal.). This is the click destination from any "Arguments" tab.
- `documents.html` — Document Reader. Always loads the TidalWave Networks v. Vellum Semiconductor § 101 Motion to Dismiss, with a surfaced-argument highlight in section III.B and the motion chain in the left panel.

## Tab bar

- Three tabs only: **Overview**, **Arguments**, **Documents**. Don't add more.
- Active tab text color is `var(--c-text-body)` (#434345). The red bottom border (`var(--c-accent)`) is the active indicator — don't tint the text red too.
- On documents.html the active tab is **Arguments** (Documents is treated as a sub-page of Arguments in the IA).
- Utility icons (folder / print / email / download) float **below** the tab divider via `position: absolute; top: 100%` on `.tab-bar`. Don't return them to the same row as the tabs.
- No breadcrumb row above the profile header. Removed in PR #8; don't add it back.

## Filters (index.html only)

- **Practice Area**: 7 items alphabetical — Civil Rights, Commercial Litigation, Contract Disputes, Employment Law, ERISA, Intellectual Property, Labor & Employment. Intellectual Property pre-checked.
- **Jurisdiction**: hierarchical drill-down — `top → Federal → 9th Circuit → S.D. Cal.` Don't return it to a flat checkbox list. Starting view is `federal-9thcir` with S.D. Cal. pre-checked.
- **Active filter pills**: `Filtered by: Intellectual Property × | S.D. Cal. ×` sits above the result cards. Clicking × removes the pill and unchecks the corresponding sidebar control (via the `juris:uncheck` CustomEvent for the Jurisdiction IIFE).
- **Results count**: `Results (6)` — must equal the number of rendered cards.
- **No real filter logic** (PRD § 12). Toggling sidebar filters does nothing. The 6 cards are static. If you ever implement filtering, filter the underlying array — never hard-code the count.

## Result cards (index.html)

- Exactly **6 cards**, all S.D. Cal., all IP-related. Source: PRD § 7.
- Order: newest-first (Mar 2024 → Sep 2022).
- **Card 1 is TidalWave Networks v. Vellum Semiconductor** — § 101 Motion to Dismiss. This is the demo's click target. Its title must link to `documents.html#argument-1`.
- Don't introduce new case names. Reuse the PRD cases: TidalWave, Helix, Marin, Sierra, Lattice, Argent.

## Document Reader (documents.html)

- Body: TidalWave § 101 brief from PRD § 9. Sections I–IV plus signature block ending `/s/ Catherine Finnerty`.
- Surfaced argument lives in section **III.B**, wrapped in `<div class="surfaced-argument-wrapper" id="argument-1">`, pale-blue (`var(--c-blue-selected)` = #eef2fa) background, with a `SURFACED ARGUMENT` label above.
- `documents.html#argument-1` must scroll the highlight into view. Inline JS handles this — native `:target` doesn't scroll inside an `overflow: auto` container.
- **Motion chain panel** (left, replaces the old document list): 4 rows in chronological order — Motion to Dismiss (selected, currently open) → Opposition → Reply → Order. Row 1 keeps its `.selected` blue background + left border. Rows 2–4 are no-op stubs that log to console.
- Reader title is capped at **100 characters** via inline JS; full text is preserved on the `title` attribute.
- Doc list panel (left) and doc reader panel (right) scroll **independently**. The page itself doesn't scroll. Don't change `html, body { overflow: hidden }` or remove `overflow-y: auto` from the panels.
- Back link reads `Back to argument list` and goes to `index.html`. Hover slides the chevron 3 px left.

## Shared chrome

- Real profile photo at `images/profile.png`, used on all three pages with `object-fit: cover` over a 70×70 circle.
- Top nav, profile header, tab bar, sidebar/filter styles, and `.tag-pill` styles all live in `styles.css`. Page-specific CSS goes in the inline `<style>` block at the top of each HTML file.

## Design system

Tokens are defined in `:root` in `styles.css`. Reuse them — don't introduce one-off colors.

- **Spacing** base: 4 px. Scale: 4, 8, 12, 16, 20, 24, 28, 40, 60.
- **Type sizes**: 12, 13, 14, 16, 20. Drift to avoid: 10, 15, 18.
- **Depth**: borders-only. Shadows only on floating overlays (citation popup).
- **Tag pill border** uses `rgba(18, 72, 160, 0.5)`. Don't return it to fully opaque primary.

Full extracted system is in `.interface-design/system.md`.

## Code organization

- **Shared CSS**: `styles.css` with CSS variable tokens in `:root`.
- **Page-specific CSS**: inline `<style>` block at the top of each HTML file.
- **No JS frameworks.** Vanilla inline JS only, kept inside `<script>` tags at the bottom of `<body>`.
- HTML is static. Deployed to GitHub Pages from `main`.

## Process rules

- One change per branch + PR. Branch off `main`.
- Squash-merge to keep `main` history linear.
- Verify visually in the preview server (`mcp__Claude_Preview__preview_start` with the `Lexis Static` config) before committing — inspect, screenshot, or both.
- Wait for the Pages deploy (`gh api /repos/.../pages/builds/latest`) before announcing the live URL.
- Don't push directly to `main`. Don't force-push.

## Out of scope

Per PRD § 11 plus what we've added since:

- Real filter logic — toggling sidebar filters does nothing; the 6 cards are static.
- Real ML output — snippets, topic tags, and motion-chain labels are hand-authored.
- Search inside the document body.
- Saving / sharing / downloading briefs.
- Pagination of the result list.
- Mobile responsive behavior.
- Authentication or user accounts.
- Back-button state preservation past browser default.
- Specialty federal courts (Tax Court, Federal Circuit drill, Bankruptcy Courts).
- State Superior Courts (the 58 county-level trial courts).
- Any secondary nav tabs not in the three-tab list.

## Reference docs in this repo

- `prd-attorney-research-flow.md` — the click-by-click PRD with all stub content (Catherine's overview, the 6 cards, the brief body, the motion chain).
- `.interface-design/system.md` — extracted design system (tokens, patterns).
