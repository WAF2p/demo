# WAF++ PASS — Web UI Demo

A fully interactive, self-contained demo of the WAF++ PASS Controls Dashboard. No server, no build step, no installation required — just open `index.html` in a browser.

## What This Is

This demo mirrors the production web UI shipped with [WAF++ PASS](../pass/) but runs entirely in the browser using embedded sample data. It is designed for:

- **CISOs and security leadership** evaluating the WAF++ framework
- **Sales and demo environments** — shareable without infrastructure
- **Onboarding** — explore controls and the waiver workflow before connecting to real IaC

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
- 17 representative WAF++ controls across 7 pillars (Security, Cost, Reliability, Operations, Sovereignty, Sustainability, Performance)
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
- 7 failures across Security, Cost, Reliability, and Sovereignty pillars
- Click any finding for full check-by-check breakdown with remediation guidance

### Compliance Matrix
- View which controls map to GDPR, ISO 27001:2022, BSI C5:2020, EUCS (ENISA), and CSRD
- Click a control card to open its detail panel

### Simulate a Scan
- Go to **Run Scan** and click **Start Demo Scan**
- The UI simulates a 1.8-second scan and refreshes the dashboard with results

## Sample Data

The demo includes a realistic scan of a fictional `demo-infrastructure/` Terraform codebase deployed in `eu-central-1` and `eu-west-1`.

| Metric | Value |
|--------|-------|
| Overall WAF++ Score | 67/100 |
| Controls Evaluated | 17 |
| Passed | 9 |
| Failed | 7 |
| Skipped | 1 |
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

## Differences from Production

| Feature | Demo | Production (`serve/`) |
|---------|------|-----------------------|
| Data source | Embedded JavaScript | Live wafpass engine |
| Scan execution | Simulated (1.8s delay) | Real in-process scan |
| Waiver persistence | In-memory only | `waivers.yml` on disk |
| Control count | 17 representative | All 70+ controls |
| API backend | None | FastAPI + uvicorn |
| Export waivers | Client-side YAML generation | Server-side `/api/waivers/export` |

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
