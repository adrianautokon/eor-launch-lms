# Cursor Handoff - Canonical Doc Flow + LMS Alignment

## Goal

Commit and deploy the updated EOR Launch LMS after Codex reorganized the local companion report pack into a cleaner canonical flow.

This handoff is for Cursor to verify, commit, and deploy. Do not rewrite the content strategy unless something is clearly broken.

## What changed

### LMS

Primary file:

- `index.html`

The LMS now mirrors the cleaned documentation hierarchy:

- `06 Legal Setup` is the parent setup module.
- `06A Entity Setup Sequence` is a child document.
- `06B Contract Stack & Signing Ops` is a child document.
- `07 Operations Lifecycle` has five lifecycle stages: `07A`, `07B`, `07C`, `07D`, `07E`.
- `07D.1 Support & People Ops Deep Dive` is nested under `07D Support & Change`, not treated as a sixth lifecycle stage.
- `08A Escalation & Dunning` is nested under `08 Coordination Playbook`.
- `11A AI Ops Stack Assessment` is nested under `11 Payments & Airwallex`.
- LMS sections now open the exact Google Doc links instead of sending learners to only the Drive folder.
- The EOR desk simulation remains a checkpoint after compliance/operations and before unit economics.

New or updated LMS sections include:

- `setup/entityseq`
- `setup/contract`
- `ops/series`
- `ops/s4deep`
- `playbook/esc`
- `pay/ai`
- refreshed home/source copy
- refreshed companion-document copy in resources

### Local DOCX pack

The cleaned local pack is outside the LMS git repo:

`../Navv_EOR_Companion_Reports_ENHANCED_DOCX_2026-07-07/`

Canonical files are now:

- `01_Opportunity.docx`
- `02_Why_Now.docx`
- `03_Market_TAM_SAM_SOM.docx`
- `04_Competition.docx`
- `05_What_Is_An_EOR.docx`
- `06_Legal_Setup.docx`
- `06A_Entity_Setup_Sequence.docx`
- `06B_Contract_Stack_Signing_Ops.docx`
- `07_Operations_Lifecycle.docx`
- `07A_Quote_and_Sign.docx`
- `07B_Onboarding.docx`
- `07C_Payroll_and_Filing.docx`
- `07D_Support_and_Change.docx`
- `07D1_Support_People_Ops_Deep_Dive.docx`
- `07E_Offboarding_and_Termination.docx`
- `08_Coordination_Playbook.docx`
- `08A_Escalation_and_Dunning.docx`
- `09_Compliance_Deep_Dive.docx`
- `10_Unit_Economics.docx`
- `11_Payments_Airwallex.docx`
- `11A_AI_Ops_Stack_Assessment.docx`
- `12_Quality_Risk_Controls.docx`
- `13_Go_To_Market.docx`
- `14_Glossary.docx`
- `15_MSA_Template_Skeleton.docx`

Also added:

- `../Navv_EOR_Companion_Reports_ENHANCED_DOCX_2026-07-07/00_Canonical_Pack_Manifest.md`

Important: these DOCX files are not currently inside the LMS git repo. If the deployment should include them as downloadable local assets, copy them into a tracked LMS folder first and update the links. Otherwise, keep the current Google Docs direct-link approach and manually upload/rename the Drive versions.

## Current git state to expect

From `eor-launch-lms`:

```bash
git status --short
```

Expected relevant changes:

- `M index.html`
- `?? CURSOR_GIT_VERCEL_PROMPT_2026-07-09_CANONICAL_DOC_FLOW.md`

There may also be an existing untracked file:

- `?? Navv_EOR_Operational_Research_Brief_2026-07-07.md`

Codex did not create or modify that research brief. Do not add it unless Diana explicitly wants it committed.

## Verification already run

Codex ran:

```bash
node - <<'NODE'
const fs=require('fs');
const html=fs.readFileSync('eor-launch-lms/index.html','utf8');
const scripts=[...html.matchAll(/<script[^>]*>([\s\S]*?)<\/script>/g)].map(m=>m[1]);
for (const [i,s] of scripts.entries()) new Function(s);
console.log('scripts ok', scripts.length);
NODE
```

Result:

- `scripts ok 2`

Codex opened every DOCX with `python-docx` and checked for stale `07F` / old "six-part" wording. Result:

- all DOCX opened successfully
- no stale live `07F` references inside DOCX body text

Codex rendered these DOCX files to PNG/PDF intermediates successfully:

- `06_Legal_Setup.docx`
- `06A_Entity_Setup_Sequence.docx`
- `06B_Contract_Stack_Signing_Ops.docx`
- `07_Operations_Lifecycle.docx`
- `07D_Support_and_Change.docx`
- `07D1_Support_People_Ops_Deep_Dive.docx`
- `08_Coordination_Playbook.docx`
- `08A_Escalation_and_Dunning.docx`
- `11_Payments_Airwallex.docx`
- `11A_AI_Ops_Stack_Assessment.docx`

Codex also served the LMS locally:

```bash
cd eor-launch-lms
python3 -m http.server 8765 --bind 127.0.0.1
```

Browser smoke checks passed for:

- `#home`
- `#setup/entityseq`
- `#setup/contract`
- `#ops/series`
- `#ops/s4deep`
- `#playbook/esc`
- `#pay/ai`
- `#resources/biz`

Observed:

- charts render
- no console errors on checked pages
- direct Google Doc links exist on the updated sections
- `07D.1` appears where expected
- stale `07F` does not appear in live LMS page text

## Cursor tasks

1. Inspect the LMS diff.

```bash
cd "/Users/dianapearson/Documents/Claude/Projects/AutoKon Product - Main/NewVenture-Services/eor-launch-lms"
git diff -- index.html
```

2. Run a local smoke test.

```bash
python3 -m http.server 8765 --bind 127.0.0.1
```

Open:

- `http://127.0.0.1:8765/index.html#home`
- `http://127.0.0.1:8765/index.html#ops/series`
- `http://127.0.0.1:8765/index.html#ops/s4deep`
- `http://127.0.0.1:8765/index.html#playbook/esc`
- `http://127.0.0.1:8765/index.html#pay/ai`

3. Commit only intended LMS changes.

```bash
git add index.html CURSOR_GIT_VERCEL_PROMPT_2026-07-09_CANONICAL_DOC_FLOW.md
git commit -m "Align LMS with canonical EOR doc flow"
```

Do not add `Navv_EOR_Operational_Research_Brief_2026-07-07.md` unless requested.

4. Deploy to Vercel.

Use the repo's existing Vercel workflow. If using CLI:

```bash
vercel --prod
```

5. After deploy, verify the production URL.

Check:

- home source copy mentions `06A/06B`, `07A-E`, `07D.1`, `08A`, and `11A`
- operations module has `4.1 People ops deep dive`
- ops section links open direct Google Docs
- playbook has `08A Escalation & dunning`
- payments has `11A AI ops stack`
- no visible `07F` label remains in the live LMS

## Manual Drive note

Diana said she will upload manually. When uploading/renaming Drive docs, use the local canonical filenames above. The current LMS uses Google document IDs, so if Drive files are replaced rather than renamed in-place, update the `DOCS` object inside `index.html` with the new document IDs before deploying.
