# Cursor Handoff: LMS Ops Direct Document Links

Use this prompt in Cursor from the LMS repo root:

```bash
cd "/Users/dianapearson/Documents/Claude/Projects/AutoKon Product - Main/NewVenture-Services/eor-launch-lms"
```

## Goal

Review, commit, push, and redeploy the latest LMS update.

This pass changes the Operations source links from folder-level Drive links to direct Google Doc links for each relevant Ops section and subsection. The LMS should still act as the synthesized learning surface, but when a learner wants the full source, they should land on the exact companion document for that stage.

## Files To Commit

```bash
git add index.html CURSOR_GIT_VERCEL_PROMPT_2026-07-08_DIRECT_DOC_LINKS.md
git commit -m "Link LMS ops sections to source docs"
git push
vercel --prod
```

Do not add unrelated local artifacts unless Diana explicitly asks.

## What Changed

1. Added a direct document registry in `index.html`
   - `DOCS.ops07` -> `07 Operations As-Is`
   - `DOCS.ops07a` -> `07A Quote & Sign`
   - `DOCS.ops07b` -> `07B Onboarding`
   - `DOCS.ops07c` -> `07C Payroll & Filing`
   - `DOCS.ops07d` -> `07D Support & Change`
   - `DOCS.ops07e` -> `07E Offboarding`
   - `DOCS.playbook08` -> `08 Coordination Playbook`
   - `DOCS.msa` -> `MSA Template Skeleton`

2. Updated the source-link helpers
   - `driveBox()` now accepts a custom URL and button label.
   - New `docBundle()` renders compact direct source-document pills.
   - `ssd()` now accepts an optional `docKey`; when passed, the SSD header opens the exact stage DOCX.

3. Updated Operations overview and series pages
   - `#ops/overview` now links directly to the overview, quote, payroll, and offboarding documents.
   - `#ops/series` now links directly to the full Ops source set instead of only the Drive folder.
   - The 07A-E table now makes each companion code clickable.

4. Updated every Ops lifecycle subsection
   - `#ops/s0` Quote & Sign opens `07A`.
   - `#ops/s1` Onboarding opens `07B`.
   - `#ops/s2` Payroll loop opens `07C`.
   - `#ops/s3` File & remit opens `07C`.
   - `#ops/s4` Support opens `07D`.
   - `#ops/s5` Offboarding opens `07E`.

5. Updated runbook/supporting Ops pages
   - `#ops/first10` links directly to `07`, `08`, and all `07A-E` stage reports.
   - `#ops/run` links directly to `07`, `08`, and the MSA skeleton.

6. Added responsive styling for the source-document bundles
   - `.doclinks` displays source docs as compact pills.
   - On mobile, the pills align left and wrap cleanly under the source-box text.

## Direct Google Doc URLs

These were resolved from the shared Drive folder:

- `07 Operations As-Is`: `https://docs.google.com/document/d/1MuTbTHucS4JcSDRKZz43FNWiZ6OJFH0_/edit`
- `07A Quote & Sign`: `https://docs.google.com/document/d/1SVCf8EsrVSKXZNtQdwStefOpiX1g1p7I/edit`
- `07B Onboarding`: `https://docs.google.com/document/d/1At2zu8BQlZCgSXkejCum-U44DOcDdPZ4/edit`
- `07C Payroll & Filing`: `https://docs.google.com/document/d/1bAih--3EaJ-QazofxoWFguH_1_FaRPMp/edit`
- `07D Support & Change`: `https://docs.google.com/document/d/11l_BsW_czUzztMv4Bm_QJd72jpsqKwGe/edit`
- `07E Offboarding`: `https://docs.google.com/document/d/1iDdYXkHTEpLpCjdzyxCsbhwCfNf6PpX5/edit`
- `08 Coordination Playbook`: `https://docs.google.com/document/d/1DO_BsxKBDtsDcWM1VGmCdJAPqQlUwmLc/edit`
- `MSA Template Skeleton`: `https://docs.google.com/document/d/1J7b3Xz2gyl10VgIYlKdK02yxisi8GKhI/edit`

The folder-level Drive URL remains available as a general full-pack fallback:

`https://drive.google.com/drive/folders/1KGw7TCS5PM2qZ6PRlBYHFbQlz2F4z_5m?usp=sharing`

## Verification Already Run

Inline script syntax check:

```bash
node - <<'NODE'
const fs=require('fs');
const html=fs.readFileSync('index.html','utf8');
const scripts=[...html.matchAll(/<script[^>]*>([\s\S]*?)<\/script>/g)].map(m=>m[1]);
for (const [i,s] of scripts.entries()) new Function(s);
const directDocLinks=(html.match(/docs\.google\.com\/document\/d\//g)||[]).length;
const folderLinks=(html.match(/drive\.google\.com\/drive\/folders/g)||[]).length;
console.log('syntax ok');
console.log('direct doc links in source:', directDocLinks);
console.log('folder fallback links in source:', folderLinks);
NODE
```

Expected:

```text
syntax ok
direct doc links in source: 8
folder fallback links in source: 1
```

## Cursor Smoke Test Before Deploy

Run:

```bash
python3 -m http.server 8768 --bind 127.0.0.1
```

Open:

- `http://127.0.0.1:8768/#ops/overview`
- `http://127.0.0.1:8768/#ops/series`
- `http://127.0.0.1:8768/#ops/s0`
- `http://127.0.0.1:8768/#ops/s1`
- `http://127.0.0.1:8768/#ops/s2`
- `http://127.0.0.1:8768/#ops/s3`
- `http://127.0.0.1:8768/#ops/s4`
- `http://127.0.0.1:8768/#ops/s5`
- `http://127.0.0.1:8768/#ops/first10`
- `http://127.0.0.1:8768/#ops/run`

Confirm:

- The Ops overview source box shows direct document pills, not just one Drive folder link.
- The Ops series table has clickable `07A`, `07B`, `07C`, `07D`, and `07E` entries.
- Each SSD header says `Open stage DOCX` and opens the right stage document.
- Each stage source box says `Open DOCX` and opens the right stage document.
- The first-10 runbook shows links to the overview, coordination playbook, and all five stage docs.
- The runbook/how-it-is-run page shows links to the overview, coordination playbook, and MSA skeleton.
- On mobile, source-document pills wrap cleanly and do not create horizontal overflow.
- The general full-pack Drive link still exists on the homepage.

## Production Check After Vercel

After `vercel --prod`, open:

- `https://eor-launch-lms.vercel.app/#ops/series`
- `https://eor-launch-lms.vercel.app/#ops/s0`
- `https://eor-launch-lms.vercel.app/#ops/s5`

Confirm:

- Direct document links open in a new tab.
- The source-document UI matches local rendering.
- No console errors or broken navigation.

## Suggested Commit Message

```text
Link LMS ops sections to source docs
```

