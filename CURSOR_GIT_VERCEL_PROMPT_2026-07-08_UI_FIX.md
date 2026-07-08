# Cursor Handoff: EOR LMS UI Fix + System Sequence Diagram

Use this prompt in Cursor from the LMS repo root:

```bash
cd "/Users/dianapearson/Documents/Claude/Projects/AutoKon Product - Main/NewVenture-Services/eor-launch-lms"
```

## Goal

Commit and redeploy the UI/UX rendering pass after the first Vercel deployment.

The deployed page showed a too-tall sticky module header on Operations: the grouped subnav stacked into three rows and consumed about 238px of vertical space. This patch makes the in-module subnav a compact horizontal rail and adds a rendered system sequence diagram to the operations series.

## Files To Commit

```bash
git add index.html CURSOR_GIT_VERCEL_PROMPT_2026-07-08_UI_FIX.md
git commit -m "Tighten LMS header and add ops sequence diagram"
git push
vercel --prod
```

Do not add `Navv_EOR_Operational_Research_Brief_2026-07-07.md` unless Diana explicitly asks for it.

## What Changed

1. Sticky header / subnav height
   - Reduced topbar padding and content top padding.
   - Changed `.subnav` from stacked, grouped rows to a single horizontal rail.
   - Removed full-width group labels in the subnav rail.
   - Added horizontal overflow for long module navs instead of wrapping into a tall header.
   - Reduced section scroll margin from `128px` to `92px`.

2. Operations navigation
   - Added `07A-E ops series` into the visible Operations subnav rail.
   - Improved active-pill horizontal centering when jumping between subsections.
   - Added measured sticky-header-aware subsection scrolling.

3. System sequence diagram
   - Added a native SVG system sequence diagram inside `#ops/series`.
   - Shows the flow from Client to Navv OS, Navv SG, Navv ID PT, Employee, and Rails/Regulators.
   - Covers quote, MSA/SOW, onboarding docs, Disnaker/BPJS/Coretax setup, Airwallex funding, payroll, filings, evidence pack, and offboarding/change request.
   - Uses inline SVG/CSS only, so no Mermaid or new runtime dependency is required.

## Verification Already Run

Inline script syntax check:

```bash
node - <<'NODE'
const fs=require('fs');
const html=fs.readFileSync('index.html','utf8');
const scripts=[...html.matchAll(/<script[^>]*>([\s\S]*?)<\/script>/g)].map(m=>m[1]);
for (let i=0;i<scripts.length;i++) new Function(scripts[i]);
console.log('ok', scripts.length);
NODE
```

Expected:

```text
ok 2
```

Local rendering check was run on:

- Desktop: `1579x925`
- Mobile: `390x844`

Observed:

- Desktop topbar: about `38px`
- Desktop subnav: about `42px`, down from the deployed `~238px`
- Mobile subnav: about `42px`
- `#ops/series` includes the system sequence diagram text and rendered SVG
- No console errors
- No visible `undefined` text

## Cursor Smoke Test Before Deploy

Run:

```bash
python3 -m http.server 8766 --bind 127.0.0.1
```

Open and inspect:

- `http://127.0.0.1:8766/#ops/overview`
- `http://127.0.0.1:8766/#ops/series`
- `http://127.0.0.1:8766/#home`

Check:

- Operations subnav is compact, one horizontal rail, not a tall stacked header.
- `7.2 07A-E ops series` appears in the Operations subnav.
- The system sequence diagram renders inside `#ops/series`.
- The diagram is horizontally scrollable on mobile.
- No chart, table, or diagram overlaps text.
- Browser console has no errors.

## Vercel Production Check

After `vercel --prod`, open:

- `https://eor-launch-lms.vercel.app/#ops/overview`
- `https://eor-launch-lms.vercel.app/#ops/series`

Confirm the production UI matches the local smoke test.
