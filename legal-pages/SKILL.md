---
name: legal-pages
description: "Use when creating or updating German legal pages for a website: Datenschutzerklärung, Impressum, AGB, Widerrufsbelehrung, and Cookie-Hinweis. Generates German-law-oriented legal page drafts with third-party tool detection and explicit legal-review warnings."
---

# Legal Pages — German Legal Page Generator

This skill generates German legal page drafts for web projects: Datenschutzerklärung, Impressum, AGB, Widerrufsbelehrung, and Cookie-Hinweis. It scans the codebase for third-party tools, gathers project-specific variables, and creates pages/components matching the project's existing patterns.

Invoke this skill when German legal pages need to be created or updated. Do not assume a specific slash-command system.

**Important:** This skill generates drafts and implementation scaffolding, not legal advice. Always recommend lawyer review before go-live.

## Agent Portability

This skill must work across Codex, Claude, and other SKILL.md-compatible agents. Do not assume a slash-command system, Claude-only project files, or a specific router/framework. Read common project instruction files when present: `AGENTS.md`, `CLAUDE.md`, `README.md`, `.cursor/rules`, `.windsurfrules`, and equivalent project docs. Use `rg`, grep, or the available search tool for detection.

## Legal Terminology

Use English for agent instructions. Generated legal page text remains German because the pages target German users and regulators. Keep German statutory terms in generated page text.

- General Data Protection Regulation (Datenschutz-Grundverordnung, DSGVO)
- Federal Data Protection Act (Bundesdatenschutzgesetz, BDSG)
- Digital Services Act (Digitale-Dienste-Gesetz, DDG)
- Telecommunications Digital Services Data Protection Act (Telekommunikation-Digitale-Dienste-Datenschutz-Gesetz, TDDDG)
- German Civil Code (Bürgerliches Gesetzbuch, BGB)
- Introductory Act to the German Civil Code (Einführungsgesetz zum Bürgerlichen Gesetzbuche, EGBGB)
- Price Indication Ordinance (Preisangabenverordnung, PAngV)
- Consumer Dispute Resolution Act (Verbraucherstreitbeilegungsgesetz, VSBG)
- Accessibility Strengthening Act (Barrierefreiheitsstärkungsgesetz, BFSG)

Do not add links to the discontinued EU Online Dispute Resolution (ODR; Online-Streitbeilegung, OS) platform. Regulation (EU) 2024/3228 repealed the ODR platform regime and the platform was discontinued on 20 July 2025.

---

## Step 0: Gather Project Info

Read common project documentation and app config to determine variables. Check `AGENTS.md`, `CLAUDE.md`, `README.md`, `.cursor/rules`, `.windsurfrules`, metadata files, route config, footer, existing legal pages, and env examples.

Required variables:

| Variable | Source | Example |
|---|---|---|
| `brandName` | Project docs/app metadata | "Fitua" |
| `websiteUrl` | Metadata/deploy config/canonical URL | "https://example.de" |
| `contactEmail` | Project docs/footer/env examples | "hello@example.de" |
| `legalEntityName` | Ask if unclear | "Example GmbH" |
| `legalForm` | Ask if unclear | "GmbH", "UG", "Einzelunternehmen" |
| `streetAddress` | Ask if unclear | "Musterstraße 1" |
| `postalCityCountry` | Ask if unclear | "10115 Berlin, Deutschland" |
| `authorizedRepresentative` | Ask if legal entity | "Max Mustermann" |
| `registerCourt` | Ask if applicable | "Amtsgericht Berlin-Charlottenburg" |
| `registerNumber` | Ask if applicable | "HRB 123456" |
| `vatId` | Ask if applicable | "DE123456789" |
| `hasPaidFeatures` | Ask if unclear | true/false |
| `hasB2C` | Ask if unclear | true/false |
| `hasNewsletter` | Scan/ask | true/false |
| `hasHealthData` | Ask if unclear | true/false |
| `hasEditorialContent` | Scan/ask | true/false |
| `regulatedProfession` | Ask if unclear | true/false/details |

If legally relevant entity data is missing, ask the user before generating final text. Never invent register, VAT, supervisory-authority, professional, or representative details.

---

## Step 1: Scan For Integrated Tools

Search source, config, package files, env examples, and public HTML for:

| Tool | Search patterns |
|---|---|
| Google Analytics | `gtag`, `GA_MEASUREMENT_ID` |
| Google Tag Manager | `GTM-`, `googletagmanager` |
| Google Ads | `google_ads`, `AW-` |
| Meta Pixel | `fbq`, `facebook`, `META_PIXEL` |
| PostHog | `posthog`, `POSTHOG_` |
| Hotjar | `hotjar`, `HOTJAR_` |
| Supabase | `supabase`, `SUPABASE_` |
| Stripe | `stripe`, `STRIPE_` |
| Brevo | `brevo`, `sendinblue` |
| Mailchimp | `mailchimp` |
| Vercel | `vercel` |
| Netlify | `netlify` |
| Cloudflare | `cloudflare`, `turnstile` |
| Make | `make.com`, `integromat` |
| YouTube/Vimeo | `youtube`, `youtu.be`, `vimeo` |
| Google Fonts | `fonts.googleapis.com`, `fonts.gstatic.com` |

Confirm the detected list with the user before generating final legal pages.

---

## Step 2: Check Project Patterns

Before editing, identify:

- Framework/router: Next.js, React Router, TanStack Router, Astro, static HTML, etc.
- Page naming and route conventions
- Layout wrappers and typography components
- Footer component and existing legal links
- Existing cookie banner or consent manager
- Existing legal pages and their structure

Match existing patterns. Do not introduce dependencies unless the user explicitly approves.

---

## Step 3: Generate Pages

Reference `legal-templates.md` in this skill directory for German text blocks. Replace placeholders with confirmed values. Include only relevant sections.

### Datenschutzerklärung (`/datenschutz` or existing project convention)

Include:

1. Verantwortlicher
2. Datenschutzbeauftragter, if applicable
3. Überblick der Verarbeitungen
4. Rechtsgrundlagen under DSGVO
5. Sicherheitsmaßnahmen
6. Speicherdauer
7. Betroffenenrechte
8. Beschwerderecht bei einer Datenschutzaufsichtsbehörde
9. Cookies and end-device access under § 25 TDDDG
10. Hosting
11. Analytics/marketing tools actually detected
12. Payment/email/auth/database/automation tools actually detected
13. Third-country transfers where applicable
14. Health/special category data only if `hasHealthData === true`

### Impressum (`/impressum`)

Include:

1. Angaben gemäß § 5 DDG
2. Contact details
3. Register/VAT details if applicable
4. Regulated-profession details if applicable
5. Responsible person under § 18 MStV if editorial content exists
6. VSBG consumer dispute-resolution statement
7. No OS/ODR platform link

### AGB (`/agb`)

Generate only when the project uses terms for services/accounts/paid features. Include:

1. Scope
2. Provider
3. Service description
4. Registration/account, if applicable
5. Contract conclusion
6. Prices/payment, if `hasPaidFeatures === true`
7. Term/cancellation, if subscriptions or accounts exist
8. Withdrawal reference for B2C paid distance contracts
9. Warranty/liability draft
10. Privacy reference
11. Final provisions

### Widerrufsbelehrung (`/widerruf` or AGB section)

Generate only if `hasB2C === true` and paid distance contracts or relevant consumer contracts exist. Use BGB/EGBGB-oriented placeholders and flag for legal review.

### Cookie-Hinweis Component

Generate a consent component when none exists or update the existing one:

- Essential category always active
- Statistics and marketing opt-in only
- Accept and reject choices equally prominent
- Granular settings
- Consent stored and revocable
- Footer link `Cookie-Einstellungen`
- Non-essential scripts blocked before consent

---

## Step 4: Routing And Footer

Add routes and footer links using the existing project pattern:

- Datenschutz -> privacy page
- Impressum -> legal notice
- AGB -> terms page, only if needed
- Widerruf -> withdrawal page, only if needed
- Cookie-Einstellungen -> reopens consent settings

---

## Rules

- German law only unless the user explicitly asks for another jurisdiction.
- Do not use Austrian laws or terms as governing rules: no ECG, FAGG, MedienG, TKG 2021, BaFG, WKO, Firmenbuchgericht, UID-Nummer.
- Do not use obsolete German primary references: TMG and TTDSG were replaced for these purposes by DDG and TDDDG.
- Generated legal page text is German; code identifiers are English unless project conventions differ.
- Always scan tools before generating a Datenschutzerklärung.
- Always confirm detected tools and legally relevant entity data.
- Do not invent legal text beyond the template library.
- Always end with: "Diese Seiten basieren auf Vorlagen und ersetzen keine Rechtsberatung. Bitte vor Go-Live rechtlich prüfen lassen."
