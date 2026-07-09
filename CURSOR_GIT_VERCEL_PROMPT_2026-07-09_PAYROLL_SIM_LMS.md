# Cursor Handoff — LMS Payroll Simulator + Compliance Alignment

Date: 2026-07-09

## Goal

Commit and deploy the updated EOR Launch OS `index.html` to GitHub and Vercel after checking the local UI. This patch aligns the LMS with the edited source DOCX pack and adds a source-backed payroll simulator learning lab in Module 9.

## What Changed

- Updated `index.html`.
- Added Module 9 subsections:
  - `9.5 Payroll validation lab`
  - `9.6 Payroll simulator lab`
- Added a richer payroll simulator covering:
  - base salary
  - fixed allowances
  - benefits/reimbursements
  - local minimum wage check
  - PKWT vs PKWTT
  - TER category A/B/C
  - JKK risk tier
  - tenure
  - Navv monthly fee and FX
  - FX buffer
  - deposit policy
  - PKWTT severance reserve
  - PHK scenario presets
  - UPH and uang pisah
  - Tapera sensitivity as watch-item only
- Simulator outputs are educational, not just numeric:
  - employee net pay
  - client monthly estimate
  - employer BPJS breakdown
  - THR liability
  - exit exposure
  - funding guardrail/deposit
  - risk flags
  - cost waterfall chart
  - learning notes explaining what moved and what still needs human validation
- Corrected stale legal wording:
  - PKWT recording and Perjanjian Alih Daya recording are separate controls.
  - The LMS no longer says late PKWT recording automatically converts the worker to PKWTT.
  - Perjanjian Alih Daya is described as recorded with local Dinas within 3 working days where the alih-daya/EOR structure is used, subject to counsel/local workflow validation.
- Added direct Drive document links for the full uploaded DOCX pack, including new child docs:
  - `06C KBLI 2025 + Permenaker Counsel Gate`
  - `06D Company Regulation & Internal Rules Pack`
  - `08B Regulatory Change Ops`
  - `09A Payroll Validation Lab`
  - `09B Payroll Simulator Spec`
  - `12A PDP Data Architecture & AI Boundary`
  - `13A Primary Research Lab`

## Drive Relinking Complete

The DOCX pack has been re-uploaded to:

https://drive.google.com/drive/folders/1KGw7TCS5PM2qZ6PRlBYHFbQlz2F4z_5m

`index.html` now maps all LMS source-document keys to the uploaded Drive document URLs, including:

```js
setup06c
setup06d
playbook08b
compliance09a
compliance09b
quality12a
gtm13a
```

No Drive relink work should remain before deployment. If a document opens in Office-compatible preview mode, that is expected because the uploaded source files are `.docx`.

## Source Basis For Simulator

The simulator should stay conservative and source-backed. It is not a payroll filing engine.

Primary/official anchors used:

- DJP TER explainer: https://pajak.go.id/id/artikel/serba-serbi-ter-pph-pasal-21-pajak-baru-atau-formula-baru
- DJP official calculator: https://kalkulator.pajak.go.id/
- BPJS Ketenagakerjaan rate explainer: https://www.bpjsketenagakerjaan.go.id/artikel/18913/artikel-berapa-besaran-iuran-jht%2C-jkk%2C-jkm%2C-jp-dan-jkp
- BPJS Kesehatan / Perpres 64/2020: https://peraturan.bpk.go.id/Details/136650/perpres-no-64-tahun-2020
- PP 35/2021: https://peraturan.bpk.go.id/Details/161904/pp-no-35-tahun-2021
- Permenaker 6/2016 THR: https://peraturan.bpk.go.id/Details/146101/permenaker-no-6-tahun-2016
- BPJS Ketenagakerjaan / PP 6/2025 JKP update: https://www.bpjsketenagakerjaan.go.id/berita/29344/Pemerintah-Optimalkan-Perlindungan-Pekerja-Lewat-PP-JKP-%26-JKK

Local source docs to keep aligned:

- `../Navv_EOR_Companion_Reports_ENHANCED_DOCX_2026-07-07/09_Compliance_Deep_Dive.docx`
- `../Navv_EOR_Companion_Reports_ENHANCED_DOCX_2026-07-07/09A_Payroll_Validation_Lab.docx`
- `../Navv_EOR_Companion_Reports_ENHANCED_DOCX_2026-07-07/09B_Payroll_Benefits_Tax_Severance_Simulator_Spec.docx`
- `../Navv_EOR_Companion_Reports_ENHANCED_DOCX_2026-07-07/07C_Payroll_and_Filing.docx`
- `../Navv_EOR_Companion_Reports_ENHANCED_DOCX_2026-07-07/10_Unit_Economics.docx`

## Local Verification Already Done

- Parsed the two inline scripts from `index.html` with Node: OK.
- Rechecked the Google Drive folder after the final upload: all new child docs are visible and directly mapped in `DOCS`.
- Checked source-document key coverage: no missing `docBundle(...)` keys and all new child docs are surfaced from the LMS.
- Served locally at `http://127.0.0.1:4173/`.
- Checked `/#compliance/simcalc` in browser:
  - no console errors
  - simulator output cards rendered
  - cost waterfall canvas rendered
  - controls updated correctly after switching to PKWT, tenure 8 months, and Tapera sensitivity
  - desktop had no body-level horizontal overflow
  - mobile 390px viewport had no body-level horizontal overflow

## Run Again Before Commit

From `eor-launch-lms`:

```bash
python3 -m http.server 4173 --bind 127.0.0.1
```

Open:

- `http://127.0.0.1:4173/#compliance/lab`
- `http://127.0.0.1:4173/#compliance/simcalc`
- `http://127.0.0.1:4173/#setup/contract`
- `http://127.0.0.1:4173/#ops/s1`

Check:

- Module 9 subsection numbering is correct.
- Simulator cards are visible and not cut off.
- Changing salary/type/tenure updates result cards.
- Tapera remains explicitly labeled as sensitivity/watch item.
- Source document links open the direct uploaded DOCX documents.
- No console errors.

## Git Caution

There is an untracked file:

```text
Navv_EOR_Operational_Research_Brief_2026-07-07.md
```

Do not add it unless Diana explicitly asks. It was pre-existing/unrelated to this LMS patch.

## Suggested Commit

```bash
git status --short
git add index.html CURSOR_GIT_VERCEL_PROMPT_2026-07-09_PAYROLL_SIM_LMS.md
git commit -m "Enhance EOR LMS compliance simulator"
```

## Deploy

This is a static single-file app. No build step is needed.

Use the project’s normal Vercel flow, for example:

```bash
vercel --prod
```

After deploy, test the live URL:

- `/#compliance/lab`
- `/#compliance/simcalc`
- `/#setup/contract`
