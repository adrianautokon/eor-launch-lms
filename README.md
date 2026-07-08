# EOR Launch OS — Global × Indonesia

A single-page "LMS to launch an EOR business" that fuses the investment case (opportunity, why-now, market, competition), the operational reality (what an EOR is, legal setup, as-is operations, coordination playbook, compliance, unit economics, payments, quality), and the execution plan (GTM, roadmap, resources, glossary).

## What this is (technically)

A **single, standalone `index.html`** — plain HTML + CSS + vanilla JS. No framework, no build step, no design-runtime, **no external dependencies**. Chart.js 4.5.0 (MIT) is vendored inline, so the file opens by double-clicking and works **fully offline** — nothing loads from a CDN at view time.

- **16 content modules** in 3 groups (**The Case** 1–4, **The Machine** 5–12, **Execute** 13–16) plus a Dashboard home — **86 routable sub-sections** with grouped sub-nav, scroll-spy, and hash deep-linking (e.g. `#compliance/sev`, `#competition` → per-competitor profile pages).
- **Coordination Playbook** module: SVG swimlane (7 actor lanes × 6 stages, 24 handoffs), RACI, manpower ladder, monthly payroll clock, termination decision tree, role-risk taxonomy, THR dashboard concept, counsel-memo checklist.
- **Glossary** module: 100+ searchable terms, contract terminology emphasized.
- **15 Chart.js charts**, dark-themed; two interactive calculators (Indonesian cost-of-employment: PPh 21 TER + BPJS + THR + severance vs Deel's $599; and a TAM/SOM slider model).
- Per-module completion + checklists persist via `localStorage` (`eorLaunchOS.v2`).
- Mobile drawer under ~760px.

> Companion research layer: the 14 DOCX reports in `../Navv_EOR_Companion_Reports_ENHANCED_DOCX_2026-07-07/` are the granular per-module research (audited 8 Jul 2026 — see `00_CLAUDE_AUDIT_AND_REVISIONS` there). The original Claude design-canvas export lives in `./_design-source/` and is not deployed.

## Deploy to Vercel

Static site, no build:

```bash
cd eor-launch-lms
vercel --prod
```

`vercel.json` sets `{"cleanUrls": true}`. Framework preset: **Other**; output = repo root.

## Editing the numbers

- **FX rate** — change `const FX = 16150;` in the app `<script>` block of `index.html` (used by the calculator and the Deel comparison).
- **PPh 21 TER bands** — edit the `const TER = { A:[...], B:[...], C:[...] }` band table near the calculator code. Each entry is `[income_ceiling_IDR, rate]`; the first band whose ceiling ≥ gross wins.
- The calculators are **directional estimates**, not filing-grade — verify against source (Ortax / Coretax) before quoting.

## Notes

- `_chart.umd.min.js` is the vendored source copy (git-ignored); the live copy is inlined in `index.html`.
- All figures trace to the sources cited in-module; regulatory items (esp. Permenaker 7/2026 fit and KBLI 78101/78300 vs 78200) need a formal legal opinion before commitment.
