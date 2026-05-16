# CCLE Court Form Assistant

**A free public legal tool by the Center for Consumer Law & Education**
Marshall University & WVU College of Law | [theccle.org](https://theccle.org)

---

## What This Is

A guided interview web application that walks West Virginia residents through the process of completing magistrate court forms. Users answer plain-language questions, optionally use an AI assistant to convert their plain-language description into formal legal claim language, and receive a completed form summary as a downloadable PDF.

The tool is designed for pro se litigants — people representing themselves — who may be unfamiliar with legal forms and court procedures.

**Live site:** [embed URL here once GitHub Pages is active]
**Embedded on:** theccle.org [page URL here]

---

## Key Design Principles

- **No backend. No database. No login.** Everything runs in the browser. No user data is stored anywhere.
- **Single HTML file.** The entire application is `index.html`. It can be opened directly in a browser, sent as an email attachment, or hosted on any static host.
- **Legal information only, not legal advice.** Disclaimer language is present throughout the tool and in every PDF output.
- **CCLE-branded.** Colors, typography, and shield mark match the CCLE visual identity (gold `#d4a020`, green `#2e8b35`, blue `#2165a8`, black `#111111`). Font is Barlow Condensed (display) and Source Sans 3 (body).

---

## Phase 1 Forms (Current)

| Form | Description |
|------|-------------|
| SCA-M207 | Civil Complaint — consumer disputes, breach of contract, property damage, unpaid debts, up to $10,000 |
| MLTPTWR / MLTAWWO | Landlord-Tenant — wrongful occupation petition (landlord) and tenant answer/defense (tenant), with branching based on user role |
| SCA-M340NP | Motion to Dismiss Traffic Citation — factual or procedural grounds |

Each form path ends with an AI-assisted statement section powered by the Anthropic Claude API (claude-sonnet-4-20250514).

---

## Technical Architecture

```
index.html
├── CSS           — All styling, CCLE brand tokens, responsive layout
├── jsPDF         — Inlined (no CDN dependency, works offline and in Safari)
├── Google Fonts  — Barlow Condensed + Source Sans 3 (graceful fallback if unavailable)
└── JavaScript
    ├── State management (plain JS object)
    ├── Form definitions (civil, landlord-tenant, traffic)
    ├── Render functions (landing, interview, review, complete)
    ├── Event binding
    ├── AI assist (Anthropic API call)
    └── PDF generation (jsPDF)
```

**Dependencies:**
- [jsPDF 2.5.1](https://github.com/parallax/jsPDF) — inlined in the HTML file
- [Anthropic Claude API](https://docs.anthropic.com) — AI assist feature only
- Google Fonts — display typography (fallback to system fonts)

**No build step required.** Edit `index.html` and deploy.

---

## AI Assist Feature

The AI assist section appears at the end of each form path. Users describe their situation in plain language; the tool calls the Anthropic API and returns 2-4 paragraphs of formal legal language appropriate for a magistrate court filing.

**API model:** `claude-sonnet-4-20250514`

**Important note on API key handling:** The current build makes the API call directly from the browser. For a public production deployment with significant traffic, the API key should be moved to a lightweight backend proxy to prevent exposure in source code and to control billing. For low-traffic use and demonstration purposes, the current approach is acceptable.

---

## Deploying / Updating

This project uses GitHub Pages for hosting.

1. Edit `index.html`
2. Commit and push to the `main` branch
3. GitHub Pages automatically deploys — changes are live within ~60 seconds
4. The Wix iFrame on theccle.org reflects the update automatically (no Wix changes needed)

---

## Roadmap

### Phase 2 — Priority Additions
- [ ] Actual WV court PDF form-fill (using pdf-lib to populate official fillable PDFs from courtswv.gov, or coordinate-based overlay for non-fillable forms)
- [ ] Expanded form library — additional magistrate court forms from the full 66-form WV courts list
- [ ] Backend API proxy for production-scale API key management
- [ ] Analytics (privacy-respecting, aggregate only — no PII)

### Phase 3 — Longer Term
- [ ] People's Law School event integration — form tool promoted at and following PLS events
- [ ] Financial EmpowerED cross-link — consumer debt and landlord-tenant content connected to Financial EmpowerED programming
- [ ] Spanish language option
- [ ] Accessibility audit (WCAG 2.1 AA compliance)
- [ ] Mobile-optimized PDF output

---

## Organizational Context

This tool was developed as a Phase 1 proof of concept for the CCLE's public legal education mission. It is modeled conceptually on the University of Houston Law Center's consumer law resources and the People's Law School format, adapted for West Virginia magistrate court users.

**CCLE Directors:**
- Damien Arthur, Marshall University (Director, MPA Program & CCLE)
- Jonathan Marshall, WVU College of Law

**Board Chair:** David J. Romano

**Strategic framing:** This tool demonstrates the CCLE's capacity to deliver accessible, technology-enabled public legal education at no cost to users. It is a proof of concept intended for demonstration to the board prior to further development and potential grant-funded expansion.

---

## Development Notes for Claude Code

This project was initiated using Claude (Anthropic) as a development assistant. The full project context — CCLE mission, programmatic priorities, peer institutions, branding, and strategic framing — is documented in the CCLE project system prompt maintained by Damien Arthur.

When continuing development with Claude Code:
- Treat `index.html` as the single source of truth
- Do not introduce a build system unless the project complexity clearly warrants it
- Maintain the no-backend, no-login, no-stored-data design constraint unless a specific feature explicitly requires otherwise
- All new form paths should follow the existing section/field definition pattern in the JavaScript form definitions
- CCLE brand tokens are defined in the `:root` CSS block — do not introduce new colors outside the established palette without approval
- Legal disclaimer language must appear on the landing page, review screen, completion screen, and in every PDF output — do not remove or abbreviate it

---

## License

This tool is a public resource produced by the Center for Consumer Law & Education. Source code is publicly available for transparency. Reuse or adaptation by other legal aid organizations is welcomed with attribution.

---

*Center for Consumer Law & Education | Marshall University & WVU College of Law | theccle.org*
