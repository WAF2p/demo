# WAF++ PASS — Web UI Demo

A fully interactive, self-contained demo of the WAF++ PASS Controls Dashboard. No server, no build step, no installation required — just open `index.html` in a browser.

## What This Is

This demo mirrors the production web UI shipped with [WAF++ PASS](../pass/) but runs entirely in the browser using embedded sample data. It is designed for:

- **CISOs and security leadership** evaluating the WAF++ framework
- **Sales and demo environments** — shareable without infrastructure
- **Onboarding** — explore controls and the waiver workflow before connecting to real IaC

The demo now includes a persisted **bright / dark theme toggle**, **11 additional intelligence views** inspired by the full product surface, and **interactive demo execution buttons** on every new view so you can simulate scans, secret detection, blast-radius analysis, remediation sprints, audit events and more without any backend.

## Quick Start

```bash
# Option 1: open directly in your browser
open index.html              # macOS
xdg-open index.html          # Linux
start index.html             # Windows

# Option 2: serve locally (avoids any browser file restrictions)
python3 -m http.server 3000  # then visit http://localhost:3000
npx serve .                  # using Node.js
```

## What You Can Do

### Browse Controls
- 20 representative WAF++ controls across 8 pillars (Security, Cost, Reliability, Operations, Sovereignty, Sustainability, Performance, Agentic)
- Filter by pillar, severity, or waiver status
- Full-text search across control IDs, titles, and descriptions
- Click any control for full details: description, regulatory mapping, automated checks

### Manage Waivers (no YAML!)
1. Find a control in the **Controls Library**
2. Click **Add Waiver**
3. Enter a reason, owner, and expiry date
4. The waiver appears immediately in the **Waivers Manager**
5. Export as `.wafpass-skip.yml` with one click

### Explore Findings
- Pre-loaded scan results from a representative AWS infrastructure scan
- 10 failures across Security, Cost, Reliability, Sovereignty, Governance, and Agentic pillars
- Click any finding for full check-by-check breakdown with remediation guidance

### Compliance Matrix
- View which controls map to GDPR, ISO 27001:2022, BSI C5:2020, EUCS (ENISA), EU AI Act, and CSRD
- Click a control card to open its detail panel

### Simulate a Scan
- Go to **Run Scan** and click **Start Demo Scan**
- The UI simulates a 1.8-second scan and refreshes the dashboard with results

### Secret Scanning (demo)
- Paste Terraform, YAML, JSON or shell snippets into **Secret Scanner**
- Click **Run Secret Scan** to simulate detection of hard-coded AWS keys, API tokens and passwords

### Blast Radius Analysis
- Open **Blast Radius** to see how each failing control cascades to downstream resources
- Click **Recalculate Blast Radius** to re-simulate the prioritisation model

### Run History & Diff
- **Run History** keeps the last 10 simulated scans
- Select a baseline and comparison run, then click **Compare Runs** to see a simulated diff of fixed vs. new failures

### Cost Impact
- **Cost Impact** estimates annual exposure, fix cost, net savings and ROI for every failing control

### Remediation Sprint
- Create a named sprint, then **Auto-Populate from Failures** to build a task list
- Mark tasks done, watch the progress bar update, and **Export Sprint CSV**

### Maturity Journey
- Browse the **Foundational → Operational → Optimised** roadmap
- Each level shows the features that are unlocked at that maturity tier

### Badges & Achievements
- Earn simulated badges for milestones such as “Zero Critical”, “Strong Posture” and “Sprint Finisher”
- Click **Recalculate** to re-evaluate against the current posture

### Evidence Locker
- **Evidence Locker** collects compliance evidence for every passing control mapped to GDPR, ISO 27001:2022, BSI C5:2020 or EUCS (ENISA)
- Click **Regenerate Evidence Pack** and **Export Markdown**

### Project Passport
- **Project Passport** generates an embeddable HTML widget summarising score, open findings, exceptions and maturity level

### Audit Log
- **Audit Log** records waiver, risk-acceptance, settings and scan events
- Click **Simulate Event** to add a random event, or **Export CSV**

### Bright / Dark Mode
- Use the theme button in the top-right header to switch between bright and dark command-center themes
- Your preference is saved in `localStorage` under the key `wafpass_theme`

## Sample Data

The demo includes a realistic scan of a fictional `demo-infrastructure/` Terraform codebase deployed in `eu-central-1` and `eu-west-1`.

| Metric | Value |
|--------|-------|
| Overall WAF++ Score | 46/100 |
| Controls Evaluated | 20 |
| Passed | 9 |
| Failed | 10 |
| Skipped | 1 |
| Waived | 0 |
| Detected Regions | eu-central-1, eu-west-1 |

### Sample failures included

| Control | Severity | Finding |
|---------|----------|---------|
| WAF-SEC-010 IAM Baseline | Critical | Password policy too weak (min 8 chars, not 14) |
| WAF-SEC-020 Encryption | Critical | S3 data lake missing encryption configuration |
| WAF-SEC-050 Logging | High | CloudTrail not multi-region |
| WAF-COST-010 Tagging | Medium | EC2 instance missing CostCenter tag |
| WAF-REL-030 Backup | High | Analytics DB backup retention only 1 day |
| WAF-SOV-020 KMS | High | KMS key rotation disabled |
| WAF-GOV-010 Governance | Low | No region-restricting Service Control Policy |
| WAF-AGENTIC-010 Agent Identity | High | Agent role shares identity and has unrestricted actions |

## Differences from Production

| Feature | Demo | Production (`serve/`) |
|---------|------|-----------------------|
| Data source | Embedded JavaScript | Live wafpass engine |
| Scan execution | Simulated (1.8s delay) | Real in-process scan |
| Secret scanning | Client-side regex simulation | gitleaks / native secret scanner |
| Blast radius | Deterministic simulation | Graph-backed dependency analysis |
| Cost impact | Estimated exposure model | Real cost data from billing APIs |
| Waiver persistence | In-memory only | `waivers.yml` on disk |
| Control count | 20 representative | All 80+ controls |
| API backend | None | FastAPI + uvicorn |
| Export waivers | Client-side YAML generation | Server-side `/api/waivers/export` |
| Theme toggle | Bright / dark command-center UI | Follows production theming |

## File Structure

```
web-ui/
├── index.html      # Complete self-contained demo (HTML + CSS + JS + data)
└── README.md       # This file
```

Everything is in a single `index.html` — no dependencies to install, no build artifacts. The file loads Tailwind CSS, Alpine.js, and Chart.js from CDN.

## Technology

- **Tailwind CSS** — utility-first CSS framework (CDN)
- **Alpine.js** — reactive UI framework (CDN)
- **Chart.js** — charts and visualisations (CDN)
- All logic in vanilla JavaScript embedded in the HTML file

## Connecting to Production

When you're ready to run against real infrastructure, deploy the internal server from the `pass/` directory:

```bash
cd ../pass
pip install fastapi uvicorn jinja2
uvicorn serve.app:app --reload --port 8080
```

The production UI has the same design and UX as this demo but connects to the live WAF++ PASS engine.
