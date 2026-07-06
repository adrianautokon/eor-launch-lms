# EOR Launch OS — Indonesia

A single-page "LMS to launch an EOR business" that fuses the investment case (opportunity, why-now, market, competition) with the operational reality (what an EOR is, setup requirements, as-is operations, compliance mechanics, unit economics, risk) and the execution plan (GTM, roadmap, resources).

## What this is (technically)

A **single, standalone `index.html`** — plain HTML + CSS + vanilla JS. No framework, no build step, no design-runtime. It opens by double-clicking the file and works fully offline **except** for Chart.js, which loads from cdnjs at view time.

- 14 content modules in 3 groups (**The Case** 1–4, **The Machine** 5–10, **Execute** 11–13) plus a Dashboard home.
- Sidebar nav + group tabs, hash-based deep-linking (e.g. `#compliance`), and a mobile drawer under ~760px.
- Per-module completion + checklists persist in the browser via `localStorage`.
- 11 Chart.js charts, dark-themed for the glass background.
- An interactive Indonesian cost-of-employment calculator (PPh 21 TER + BPJS + THR + severance, vs Deel's $599).

> This is the consolidated production build. The original Claude design-canvas export (the `.dc.html` + its React `support.js` runtime) lives in `./_design-source/` and is **not** deployed — it's git-ignored and vercel-ignored. The production file carries that visual design with no runtime dependency.

## Deploy to Vercel

Static site, no build:

```bash
cd eor-launch-lms
vercel --prod
```

`vercel.json` sets `{"cleanUrls": true}`. Framework preset: **Other**; output = repo root.

## Editing the numbers

- **FX rate** — change `const FX = 16150;` in the `<script>` block of `index.html` (used by the calculator and the Deel comparison).
- **PPh 21 TER bands** — edit the `const TER = { A:[...], B:[...], C:[...] }` band table just below the calculator code. Each entry is `[income_ceiling_IDR, rate]`; the first band whose ceiling ≥ gross wins.
- The calculator is a **directional estimate**, not filing-grade — verify against source (Ortax / Coretax) before quoting.

## Notes

- Progress (module completion + checklists) persists per-browser in `localStorage` under key `eorLaunchOS.v1`.
- All figures trace to the sources cited in the Resources module; regulatory items need a formal legal opinion before commitment.
