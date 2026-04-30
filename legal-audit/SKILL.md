---
name: legal-audit
description: "Audit website legal compliance for Germany: General Data Protection Regulation (Datenschutz-Grundverordnung, DSGVO), Federal Data Protection Act (Bundesdatenschutzgesetz, BDSG), Digital Services Act (Digitale-Dienste-Gesetz, DDG) Impressum duties, Telecommunications Digital Services Data Protection Act (Telekommunikation-Digitale-Dienste-Datenschutz-Gesetz, TDDDG) cookie consent, German consumer law, pricing, dispute-resolution notices, accessibility, and third-country transfers."
---

# Legal Audit — German Website Compliance

This skill audits a website for legal compliance under German and EU law. It checks mandatory legal pages, privacy disclosures, cookie consent, form handling, third-party data transfers, e-commerce requirements, dispute-resolution notices, accessibility requirements, and security-related technical measures.

Invoke this skill when a German website legal compliance audit is requested. It should scan the project automatically without assuming a specific slash-command system.

## Agent Portability

This skill must work across Codex, Claude, and other SKILL.md-compatible agents. Do not assume slash commands, Claude-only memory files, or a specific tool stack. If a related audit skill is unavailable, mention the missing follow-up check instead of invoking it.

## Important Boundary

This skill provides technical compliance checks, not legal advice. Clearly separate implementation fixes from legal content that needs lawyer review.

## Reference Sources And Standard Terms

Use English for agent instructions. Keep German statutory terms in generated reports and legal-page labels where users/regulators expect them.

- General Data Protection Regulation (Datenschutz-Grundverordnung, DSGVO; Regulation (EU) 2016/679)
- Federal Data Protection Act (Bundesdatenschutzgesetz, BDSG)
- Digital Services Act (Digitale-Dienste-Gesetz, DDG), especially § 5 DDG for Impressum/provider identification
- Telecommunications Digital Services Data Protection Act (Telekommunikation-Digitale-Dienste-Datenschutz-Gesetz, TDDDG), especially § 25 TDDDG for cookies/end-device access
- German Civil Code (Bürgerliches Gesetzbuch, BGB), especially §§ 312g and 355 BGB for consumer withdrawal rights
- Introductory Act to the German Civil Code (Einführungsgesetz zum Bürgerlichen Gesetzbuche, EGBGB), especially Art. 246a EGBGB for consumer information duties
- Price Indication Ordinance (Preisangabenverordnung, PAngV)
- Consumer Dispute Resolution Act (Verbraucherstreitbeilegungsgesetz, VSBG), especially § 36 VSBG
- Accessibility Strengthening Act (Barrierefreiheitsstärkungsgesetz, BFSG), implementing the European Accessibility Act (EAA)
- Regulation (EU) 2024/3228 discontinued the EU Online Dispute Resolution platform on 20 July 2025. Do not add new OS/ODR platform links.

---

## Step 0: Understand The Project

Before auditing, read:

- Project docs: `AGENTS.md`, `CLAUDE.md`, `README.md`, `.cursor/rules`, `.windsurfrules`, or equivalent
- All legal pages: Impressum, Datenschutzerklärung, AGB, Widerrufsbelehrung, Cookie settings
- Cookie consent implementation
- Forms and data collection points
- Third-party scripts and services: analytics, fonts, CDNs, APIs, embeds, payment, email
- Hosting, server region, and data-transfer configuration
- Security headers and HTTPS setup

Identify:

- **Business type:** B2C, B2B, mixed, editorial/media, regulated profession, e-commerce, SaaS, marketplace
- **Legal entity:** GmbH, UG, Einzelunternehmen, foreign entity, freelancer, etc.
- **Data flows:** what personal data is collected, where it goes, and which processors are involved
- **Consent mechanism:** how cookies/tracking/embeds are handled
- **Consumer flow:** checkout, subscriptions, paid features, digital services, withdrawal rights

---

## Step 1: Audit Categories

For every issue found, note the file, legal basis, severity, and recommended fix.

Severity:

- **Critical:** likely legal violation, fine risk, Abmahnung risk, or unlawful tracking/data transfer
- **Major:** incomplete compliance; should fix before launch
- **Minor:** best practice or clarity improvement

### Category 1: Mandatory Legal Pages

#### Impressum — § 5 DDG, plus § 18 MStV when editorial content exists

Check for:

- Impressum page exists and is linked from every page, typically footer
- Link label is clearly `Impressum`
- Information is easily recognizable, directly accessible, and permanently available
- Required fields are present where applicable:
  - Name/company and full address
  - Legal form and authorized representative for legal entities
  - Fast electronic contact, including email
  - Commercial register, association register, partnership register, or cooperative register plus registration number if applicable
  - VAT ID (USt-IdNr.) or business identification number if applicable
  - Supervisory authority if activity requires authorization
  - Professional chamber, professional title, state of award, and professional rules for regulated professions
  - Responsible person under § 18 MStV for journalistic-editorial content

Fix pattern: create/update Impressum with project-specific legal entity data. Do not invent register numbers or professional-regulation details; ask the user or flag for legal review.

#### Datenschutzerklärung — Art. 12-14 DSGVO, BDSG where applicable

Check for:

- Privacy policy exists and is linked from every page
- Linked near every form that collects personal data
- Contains:
  - Controller (Verantwortlicher) with contact details
  - Data protection officer if required
  - Purposes and legal bases per processing activity
  - Recipients or categories of recipients
  - Third-country transfers and safeguards
  - Retention periods or criteria
  - Data subject rights
  - Right to lodge a complaint with a supervisory authority
  - Cookies, analytics, embeds, payment, email, hosting, and other processors
  - Whether data provision is statutory/contractual/required

Fix pattern: update technical links and detected processing inventory. Do not invent legal clauses; flag content for legal review unless using an approved template.

#### AGB — Terms and Conditions

Check when products, services, accounts, subscriptions, or paid features exist:

- AGB page exists if terms are used
- Terms are accessible before contract conclusion
- Checkout or signup requires legally appropriate acknowledgement where needed
- Terms cover scope, services, contract conclusion, prices/payment, account rules, term/termination, liability, warranty, data protection reference, governing law, and consumer protections
- No clauses obviously hostile to German consumer law

Fix pattern: add missing links and acceptance mechanics. Flag substantive clauses for legal review.

#### Widerrufsbelehrung — §§ 312g, 355 BGB; Art. 246a EGBGB

Check for B2C distance contracts:

- Withdrawal information exists before contract conclusion
- 14-day withdrawal period is stated where applicable
- Model withdrawal form is provided where required
- Exceptions or expiry rules are stated only when applicable
- Digital-content/service early-performance consent is explicit if relied on

Fix pattern: add missing page/link/checkout placement. Flag final wording for legal review.

---

### Category 2: Cookies, Tracking, And End-Device Access — § 25 TDDDG + DSGVO

Check for:

- Non-essential cookies, localStorage, SDK identifiers, pixels, embeds, and similar technologies are blocked until consent
- Consent is opt-in, granular, specific, informed, revocable, and documented
- Reject is as easy as accept
- No pre-ticked non-essential categories
- Consent settings can be reopened from footer or privacy page
- Google Analytics, GTM, Ads, Meta Pixel, Hotjar, PostHog, YouTube/Vimeo embeds, social embeds, chat widgets, and marketing tools do not load before consent unless truly essential
- Cookie/privacy documentation matches actual storage and network behavior

Fix pattern: implement consent-gated loading; add or fix cookie settings UI; remove pre-consent tracking.

---

### Category 3: Forms And Data Processing

Check every form:

- Privacy policy link near the form
- Required fields are minimized
- Contact forms use a plausible legal basis and do not require unnecessary consent checkboxes
- Newsletter signup uses explicit consent, double opt-in, unsubscribe, and proof of consent
- Account/payment forms describe contractual necessity
- Health/sensitive data uses explicit Art. 9 DSGVO basis where applicable
- Error and success states do not leak personal data

Fix pattern: add privacy links, remove unnecessary required fields, add consent/DOI for newsletters, and associate consent records with timestamps/source.

---

### Category 4: Third-Country Transfers And Processors

Check for:

- All processors are identified: hosting, analytics, email, payment, CRM, automation, support, CDN, database, auth
- Data processing agreements (AVV/DPA) are required and flagged for manual verification
- Transfers outside EU/EEA are disclosed with safeguards: adequacy decision, EU-US Data Privacy Framework certification, Standard Contractual Clauses, and transfer impact assessment where relevant
- US services are consent-gated when used for non-essential tracking
- Google Fonts and similar assets are self-hosted where possible

Fix pattern: gate or remove unnecessary third-country transfers, self-host static assets, and document processors. Flag AVV/DPA and transfer-impact issues for manual/legal review.

---

### Category 5: HTTPS And Security Headers

Check for:

- HTTPS everywhere and HTTP to HTTPS redirect
- No mixed content
- HSTS where appropriate
- `Content-Security-Policy`
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options` or CSP `frame-ancestors`
- `Referrer-Policy`
- `Permissions-Policy`

Fix pattern: add headers via hosting config, middleware, or static `_headers` file. Use a dedicated security audit skill if deeper code security review is needed.

---

### Category 6: Retention And Documentation

Check for:

- Specific retention periods in privacy policy
- Server logs retention defined
- Contact form inquiries deletion period defined
- Newsletter deletion/unsubscribe behavior defined
- Account deletion path or manual process defined
- Processing records (Verzeichnis von Verarbeitungstätigkeiten, VVT) flagged for manual review where required

Fix pattern: define retention behavior in code/config where possible; flag legal documentation for manual review.

---

### Category 7: E-Commerce And Consumer Law

Check if B2C, e-commerce, subscriptions, paid digital services, or checkout exist:

- Prices show total price including VAT and other price components under PAngV
- Shipping/delivery/additional costs are shown before checkout where applicable
- Button text clearly indicates payment obligation, e.g. `zahlungspflichtig bestellen`
- Order summary and correction option exist before final order
- Confirmation email or durable-medium confirmation is sent where applicable
- AGB, privacy policy, and withdrawal information are available before purchase
- Subscription terms, renewal, cancellation, and minimum term are transparent

Fix pattern: update checkout UI, price labels, links, and confirmation flow.

---

### Category 8: Dispute Resolution — § 36 VSBG; ODR Repeal

Check for:

- Website and AGB state whether the business is willing or obliged to participate in consumer dispute-resolution proceedings before a Verbraucherschlichtungsstelle, unless an exception applies
- If obliged or willing, the responsible consumer arbitration body is named with address and website
- No obsolete OS/ODR platform link remains; the EU ODR platform was discontinued on 20 July 2025

Fix pattern: remove obsolete OS/ODR platform links. Add or flag VSBG participation statement for legal review.

---

### Category 9: Accessibility — BFSG / EAA

Check for:

- Whether the service falls under BFSG scope, especially e-commerce and consumer-facing digital services
- Accessibility statement exists if required or useful
- Basic indicators: semantic HTML, keyboard navigation, contrast, alternative text, form labels

Fix pattern: flag BFSG scope and use the accessibility audit skill for full WCAG testing.

---

## Step 2: Report Findings

Use this format:

```markdown
## Legal Audit Results — Germany

### Critical
| Issue | Legal Basis | File / Location | Required Fix |
|---|---|---|---|

### Major
| Issue | Legal Basis | File / Location | Required Fix |
|---|---|---|---|

### Minor
| Issue | Legal Basis | File / Location | Required Fix |
|---|---|---|---|

### Manual Legal Review Required
- [ ] Impressum entity details
- [ ] Datenschutzerklärung wording
- [ ] AGB clauses
- [ ] Widerrufsbelehrung and model withdrawal form
- [ ] AVV/DPA agreements
- [ ] VVT / processing records

### Compliant
- [list what is already sound]
```

Ask the user: **"Should I fix the technical issues? Legal page content, AGB, withdrawal text, and AVV/DPA agreements still need legal review."**

---

## Step 3: Fix Technical Issues

Can fix:

- Missing footer links
- Privacy links near forms
- Consent-gated scripts
- Cookie settings UI
- Google Fonts self-hosting
- Newsletter double opt-in mechanics
- Checkout link placement and payment-button text
- Security headers

Cannot independently finalize:

- Legal entity details
- AGB legal clauses
- Privacy policy legal wording
- Withdrawal wording
- AVV/DPA agreements
- VVT / processing records
- Regulated-profession disclosures

---

## Rules

- German law first: DDG, TDDDG, BDSG, BGB, EGBGB, PAngV, VSBG, BFSG, plus directly applicable EU law such as DSGVO/GDPR.
- Do not use Austrian-specific laws such as ECG, FAGG, MedienG, TKG 2021, BaFG, WKO, Firmenbuchgericht, or UID-Nummer unless the user explicitly asks for Austrian compliance.
- Do not use obsolete German references such as TMG or TTDSG as current primary law; prefer DDG and TDDDG.
- Do not add OS/ODR platform links; remove obsolete links after 20 July 2025.
- Reference specific legal articles for every finding.
- Do not present generated legal text as legal advice.
- Follow the repository's existing code style and component patterns.
