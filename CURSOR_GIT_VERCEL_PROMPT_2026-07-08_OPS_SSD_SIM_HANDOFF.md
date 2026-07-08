# Cursor handoff: LMS ops SSDs, Drive source links, and EOR desk simulation checkpoint

You are working in:

`/Users/dianapearson/Documents/Claude/Projects/AutoKon Product - Main/NewVenture-Services/eor-launch-lms`

## Goal

Review, commit, push, and deploy the latest `index.html` enhancement to GitHub and Vercel.

This pass aligns the LMS more tightly with the enhanced DOCX report pack while keeping the LMS as the synthesized course surface.

## Files to include

- `index.html`
- `CURSOR_GIT_VERCEL_PROMPT_2026-07-08_OPS_SSD_SIM_HANDOFF.md`

Do not accidentally add unrelated local artifacts such as:

- `Navv_EOR_Operational_Research_Brief_2026-07-07.md`

## What changed

1. Added compact SSD blocks to every Operations sub-stage:
   - `07A SSD · Quote & Sign`
   - `07B SSD · Onboarding`
   - `07C SSD · Payroll Run`
   - `07C SSD · Filing & Remittance`
   - `07D SSD · Support & Change`
   - `07E SSD · Offboarding & Termination`

2. Added Google Drive source links:
   - Homepage source-pack link
   - Operations source-bundle link
   - Per-stage DOCX source links inside the ops SSD sections
   - Drive URL: `https://drive.google.com/drive/folders/1KGw7TCS5PM2qZ6PRlBYHFbQlz2F4z_5m?usp=sharing`

3. Added university-module framing around the operations series:
   - Module outcomes
   - Lab artifacts
   - Assessment gates
   - Source-bundle cue

4. Added the EOR desk simulation as a first-class checkpoint before Unit Economics:
   - New sidebar module: `9.5 EOR Desk Sim`
   - New route: `#sim/play`
   - Unit Economics `#econ/pl` starts with the same simulation checkpoint
   - Simulation URL: `https://navv-desk-sim.vercel.app/`

5. Added responsive styling for:
   - `.ssd` SSD sequence cards
   - `.drivebox` source-pack cards
   - `.simbox` simulation checkpoint cards

## Why this placement

The sim belongs after Operations + Compliance and before Unit Economics. Learners should first understand the lifecycle, statutory gates, payroll proof, and termination risk; then they should play the scenario quiz; then they should study pricing, staffing, break-even, and margin.

## Local verification already run

Syntax check:

```bash
node - <<'NODE'
const fs=require('fs');
const html=fs.readFileSync('index.html','utf8');
const scripts=[...html.matchAll(/<script[^>]*>([\s\S]*?)<\/script>/g)].map(m=>m[1]);
for (let i=0;i<scripts.length;i++) new Function(scripts[i]);
console.log('ok scripts', scripts.length);
NODE
```

Expected:

```text
ok scripts 2
```

Link check:

```bash
curl -I -L --max-time 15 https://navv-desk-sim.vercel.app/
```

Expected:

```text
HTTP/2 200
```

Browser smoke checks were also run against a local static server. Verified:

- `#sim/play` renders the sim checkpoint and CTA
- `#econ/pl` renders the sim checkpoint before the P&L section
- `#ops/series` includes the operations source bundle
- `#ops/s0` includes `07A SSD`
- `#ops/s5` includes `07E SSD`
- Desktop and mobile checks showed `horizontalOverflow: 0`

## Suggested Cursor QA before commit

Run:

```bash
python3 -m http.server 8767 --bind 127.0.0.1
```

Open:

- `http://127.0.0.1:8767/index.html#sim/play`
- `http://127.0.0.1:8767/index.html#econ/pl`
- `http://127.0.0.1:8767/index.html#ops/series`
- `http://127.0.0.1:8767/index.html#ops/s0`
- `http://127.0.0.1:8767/index.html#ops/s5`

Confirm:

- The header height remains compact.
- No horizontal scrolling appears on desktop or mobile.
- The sim appears in the sidebar between Compliance Deep-Dive and Unit Economics.
- The sim CTA opens `https://navv-desk-sim.vercel.app/`.
- Drive links open the shared source folder.
- SSD cards are readable and do not crowd the existing lesson text.

## Suggested commit

```bash
git add index.html CURSOR_GIT_VERCEL_PROMPT_2026-07-08_OPS_SSD_SIM_HANDOFF.md
git commit -m "Enhance LMS ops SSDs and simulation checkpoint"
git push
```

Then deploy through the existing Vercel flow for this project.

