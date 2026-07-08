# Cursor Handoff: Align EOR Launch LMS With Enhanced Claude Report Pack

Use this prompt in Cursor from the repo root:

```bash
cd "/Users/dianapearson/Documents/Claude/Projects/AutoKon Product - Main/NewVenture-Services/eor-launch-lms"
```

## Goal

Review, commit, push, and deploy the enhanced static LMS to Vercel. The LMS is `index.html` and is now aligned with the local enhanced Claude report pack:

```text
../Navv_EOR_Companion_Reports_ENHANCED_DOCX_2026-07-07/
```

## Files To Commit

Commit these two files:

```bash
git add index.html CURSOR_GIT_VERCEL_PROMPT_2026-07-08.md
git commit -m "Align EOR LMS with enhanced Claude reports"
```

Do not add the currently untracked `Navv_EOR_Operational_Research_Brief_2026-07-07.md` unless Diana explicitly asks for it.

## What Changed In `index.html`

- Aligned the LMS with the 8 Jul 2026 Claude audit and Drive-comment response map.
- Added a visible "Latest companion-pack alignment" layer on the dashboard.
- Added the new `#ops/series` section for the 07A-E ops series:
  - 07A Quote & Sign
  - 07B Onboarding
  - 07C Payroll & Filing
  - 07D Support & Change
  - 07E Offboarding
- Updated legal/setup from the older 78200-first assumption to the newer Memo B posture:
  - KBLI 78101 likely primary
  - KBLI 78300 possible secondary
  - KBLI 78200 treated as the conflict/blocked case
  - Kontrak Hukum / counsel Memos A/B/C before incorporation spend
- Updated unit economics:
  - Regus/Ciputra office quote treated as a licensing/KYC asset
  - ~$3.9k fixed base
  - ~$56k launch trough
  - ~27-seat break-even
  - $179 as floor, $249 as base-case calculator default, $299 as premium test
- Updated competition and GTM:
  - SEA/APAC buyer geography expanded to SG, JP, KR, TW/HK plus AU/UK/EU and US async buyers
  - local price reality shown as roughly $300-500/mo
  - employment rails first, recruitment only later if demand pulls it
- Updated payments:
  - Airwallex primary global collection/conversion rail
  - Xendit as Indonesia-side disbursement alternative
  - SG-to-ID intercompany and transfer-pricing documentation flagged
- Updated resources copy to list the enhanced pack, 07A-E ops docs, MSA skeleton, audit, and response map.

## Verification Already Run

Inline script syntax check:

```bash
node - <<'NODE'
const fs=require('fs');
const html=fs.readFileSync('index.html','utf8');
const scripts=[...html.matchAll(/<script[^>]*>([\s\S]*?)<\/script>/g)].map(m=>m[1]);
console.log('scripts', scripts.length, scripts.map(s=>s.length));
for (let i=0;i<scripts.length;i++) new Function(scripts[i]);
console.log('ok');
NODE
```

Expected:

```text
scripts 2 [...]
ok
```

## Cursor Checks Before Commit

Run:

```bash
git status --short
python3 -m http.server 8765 --bind 127.0.0.1
```

Open:

- `http://127.0.0.1:8765/#home`
- `http://127.0.0.1:8765/#ops/series`
- `http://127.0.0.1:8765/#setup/seq`
- `http://127.0.0.1:8765/#econ/be`
- `http://127.0.0.1:8765/#gtm/sg`
- `http://127.0.0.1:8765/#pay/flow`

Smoke-test that charts render and navigation resolves without console errors.

## Push And Deploy

After commit:

```bash
git status --short
git push
vercel --prod
```

After deployment, manually test the production Vercel URL:

- `/#home`
- `/#ops/series`
- `/#ops/first10`
- `/#setup/seq`
- `/#econ/calc`
- `/#roadmap/check`

## Google Drive / DOCX Note

Do not upload anything to Google Drive from Cursor. Diana will upload manually.

The local DOCX source pack remains:

```text
../Navv_EOR_Companion_Reports_ENHANCED_DOCX_2026-07-07/
```
