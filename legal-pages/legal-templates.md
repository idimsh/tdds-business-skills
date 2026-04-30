# Legal Templates — German Legal Page Text Blocks

Reference file for the `legal-pages` skill. Contains German-law-oriented text blocks and page section templates for German legal page drafts.

**Applicable law:** General Data Protection Regulation (Datenschutz-Grundverordnung, DSGVO), Federal Data Protection Act (Bundesdatenschutzgesetz, BDSG), Digital Services Act (Digitale-Dienste-Gesetz, DDG), Telecommunications Digital Services Data Protection Act (Telekommunikation-Digitale-Dienste-Datenschutz-Gesetz, TDDDG), German Civil Code (Bürgerliches Gesetzbuch, BGB), Introductory Act to the German Civil Code (Einführungsgesetz zum Bürgerlichen Gesetzbuche, EGBGB), Price Indication Ordinance (Preisangabenverordnung, PAngV), Consumer Dispute Resolution Act (Verbraucherstreitbeilegungsgesetz, VSBG), Accessibility Strengthening Act (Barrierefreiheitsstärkungsgesetz, BFSG).

**Language rule:** Agent instructions and explanatory headings are English. Legal page text blocks remain German because they are intended for German legal pages.

**Current ODR/OS platform rule:** Do not include links to the EU Online Dispute Resolution (ODR; Online-Streitbeilegung, OS) platform. Regulation (EU) 2024/3228 repealed the ODR platform regime and the platform was discontinued on 20 July 2025.

**Required placeholders:**

| Placeholder | Description | Example |
|---|---|---|
| `{{brandName}}` | Product/brand name | Fitua |
| `{{websiteUrl}}` | Full website URL | https://example.de |
| `{{contactEmail}}` | Project contact email | hello@example.de |
| `{{legalEntityName}}` | Legal entity / provider | Example GmbH |
| `{{legalForm}}` | Legal form | GmbH |
| `{{streetAddress}}` | Street address | Musterstraße 1 |
| `{{postalCityCountry}}` | Postal code, city, country | 10115 Berlin, Deutschland |
| `{{authorizedRepresentative}}` | Authorized representative | Max Mustermann |
| `{{registerCourt}}` | Register court, if applicable | Amtsgericht Berlin-Charlottenburg |
| `{{registerNumber}}` | Register number, if applicable | HRB 123456 |
| `{{vatId}}` | VAT ID, if applicable | DE123456789 |
| `{{currentDate}}` | Date of generation | 30.04.2026 |

---

## 1. Datenschutzerklärung — Section Templates

### 1.1 Verantwortlicher

```text
Verantwortlich für die Datenverarbeitung auf {{websiteUrl}} ist:

{{legalEntityName}}
{{streetAddress}}
{{postalCityCountry}}
E-Mail: {{contactEmail}}

Vertreten durch: {{authorizedRepresentative}}

Bei Fragen zum Datenschutz erreichen Sie uns unter {{contactEmail}}.
```

### 1.2 Datenschutzbeauftragter (Conditional)

```text
Datenschutzbeauftragter

Unseren Datenschutzbeauftragten erreichen Sie unter:
{{dataProtectionOfficerNameOrCompany}}
{{dataProtectionOfficerAddress}}
E-Mail: {{dataProtectionOfficerEmail}}
```

### 1.3 Überblick der Verarbeitungen

```text
Wir verarbeiten personenbezogene Daten, soweit dies zur Bereitstellung dieser Website, zur Kommunikation mit Nutzern, zur Erbringung unserer Leistungen und zur Erfüllung gesetzlicher Pflichten erforderlich ist.

Folgende Kategorien personenbezogener Daten können verarbeitet werden:
- Bestandsdaten, z. B. Name und Kontaktdaten
- Kommunikationsdaten, z. B. E-Mail-Adresse und Nachrichteninhalte
- Nutzungsdaten, z. B. besuchte Seiten, Zugriffszeiten und Interaktionen
- Technische Daten, z. B. IP-Adresse, Browsertyp, Betriebssystem und Geräteinformationen
- Vertrags- und Zahlungsdaten, sofern kostenpflichtige Leistungen genutzt werden

Die konkreten Verarbeitungen hängen davon ab, welche Funktionen von {{brandName}} genutzt werden.
```

### 1.4 Rechtsgrundlagen

```text
Die Verarbeitung personenbezogener Daten erfolgt auf Grundlage der Datenschutz-Grundverordnung (DSGVO):

- Art. 6 Abs. 1 lit. a DSGVO: Einwilligung, insbesondere für nicht erforderliche Cookies, Analyse- und Marketingdienste sowie Newsletter.
- Art. 6 Abs. 1 lit. b DSGVO: Vertragserfüllung oder vorvertragliche Maßnahmen, insbesondere bei Nutzerkonten, Bestellungen, Zahlungen oder der Nutzung unserer Dienste.
- Art. 6 Abs. 1 lit. c DSGVO: Erfüllung rechtlicher Verpflichtungen, insbesondere handels- und steuerrechtliche Aufbewahrungspflichten.
- Art. 6 Abs. 1 lit. f DSGVO: Berechtigte Interessen, insbesondere sicherer Betrieb, Missbrauchsprävention, technische Stabilität und Beantwortung allgemeiner Anfragen.

Soweit besondere Kategorien personenbezogener Daten verarbeitet werden, erfolgt dies nur auf einer Rechtsgrundlage nach Art. 9 DSGVO.
```

### 1.5 Cookies and End-Device Access

```text
Cookies und ähnliche Technologien

Unsere Website verwendet Cookies und ähnliche Technologien. Der Zugriff auf Informationen in Endeinrichtungen und die Speicherung von Informationen in Endeinrichtungen richten sich nach § 25 Telekommunikation-Digitale-Dienste-Datenschutz-Gesetz (TDDDG).

Technisch erforderliche Cookies und Speicherzugriffe werden eingesetzt, um die Website bereitzustellen und grundlegende Funktionen zu ermöglichen. Nicht erforderliche Cookies, Analyse- und Marketingdienste setzen wir nur mit Ihrer Einwilligung ein.

Sie können Ihre Einwilligung jederzeit über die Cookie-Einstellungen widerrufen oder ändern.
```

### 1.6 Speicherdauer

```text
Speicherdauer

Wir speichern personenbezogene Daten nur so lange, wie dies für die jeweiligen Zwecke erforderlich ist oder gesetzliche Aufbewahrungspflichten bestehen. Danach werden die Daten gelöscht oder anonymisiert.

Server-Logdaten werden regelmäßig nach einem kurzen Zeitraum gelöscht, sofern keine längere Speicherung zur Aufklärung von Sicherheitsvorfällen erforderlich ist. Vertrags- und Zahlungsdaten werden entsprechend den gesetzlichen Aufbewahrungspflichten gespeichert.
```

### 1.7 Betroffenenrechte

```text
Ihre Rechte

Sie haben nach der DSGVO insbesondere folgende Rechte:

- Recht auf Auskunft (Art. 15 DSGVO)
- Recht auf Berichtigung (Art. 16 DSGVO)
- Recht auf Löschung (Art. 17 DSGVO)
- Recht auf Einschränkung der Verarbeitung (Art. 18 DSGVO)
- Recht auf Datenübertragbarkeit (Art. 20 DSGVO)
- Recht auf Widerspruch (Art. 21 DSGVO)
- Recht auf Widerruf erteilter Einwilligungen (Art. 7 Abs. 3 DSGVO)

Zur Ausübung Ihrer Rechte können Sie sich jederzeit an {{contactEmail}} wenden.
```

### 1.8 Beschwerderecht

```text
Beschwerderecht

Sie haben das Recht, sich bei einer Datenschutzaufsichtsbehörde zu beschweren, wenn Sie der Ansicht sind, dass die Verarbeitung Ihrer personenbezogenen Daten gegen Datenschutzrecht verstößt.

Sie können sich insbesondere an die Datenschutzaufsichtsbehörde Ihres Wohnortes oder an die für uns zuständige Aufsichtsbehörde wenden.
```

### 1.9 Third-Country Transfers

```text
Drittlandübermittlungen

Sofern personenbezogene Daten in Länder außerhalb der Europäischen Union oder des Europäischen Wirtschaftsraums übermittelt werden, erfolgt dies nur, wenn hierfür eine Rechtsgrundlage besteht. Dies kann insbesondere ein Angemessenheitsbeschluss der Europäischen Kommission, eine Zertifizierung nach dem EU-US Data Privacy Framework, Standardvertragsklauseln der Europäischen Kommission oder eine ausdrückliche Einwilligung sein.
```

### 1.10 Health Data (Conditional: hasHealthData)

```text
Besondere Kategorien personenbezogener Daten

Soweit im Rahmen von {{brandName}} Gesundheitsdaten oder andere besondere Kategorien personenbezogener Daten im Sinne von Art. 9 DSGVO verarbeitet werden, erfolgt dies nur auf Grundlage einer ausdrücklichen Einwilligung oder einer anderen einschlägigen Rechtsgrundlage nach Art. 9 DSGVO.

Wir treffen zusätzliche technische und organisatorische Schutzmaßnahmen, insbesondere Verschlüsselung, Zugriffsbeschränkungen und eine strenge Zweckbindung.
```

---

## 2. Tool-Specific Privacy Texts

### 2.1 Hosting — Generic

```text
Hosting

Unsere Website wird bei {{hostingProvider}} betrieben. Beim Aufruf der Website verarbeitet der Hostinganbieter technische Zugriffsdaten, insbesondere IP-Adresse, Zeitpunkt des Zugriffs, aufgerufene Seite, übertragene Datenmenge, Browser- und Betriebssysteminformationen.

Rechtsgrundlage ist Art. 6 Abs. 1 lit. f DSGVO. Unser berechtigtes Interesse liegt in der sicheren, stabilen und effizienten Bereitstellung der Website.

Mit dem Anbieter besteht, soweit erforderlich, ein Vertrag über Auftragsverarbeitung nach Art. 28 DSGVO.
```

### 2.2 Google Analytics / GA4

```text
Google Analytics

Wir verwenden Google Analytics, einen Webanalysedienst der Google Ireland Limited, Gordon House, Barrow Street, Dublin 4, Irland. Google Analytics hilft uns zu verstehen, wie Besucher unsere Website nutzen.

Google Analytics wird nur nach Ihrer Einwilligung geladen. Rechtsgrundlage ist Art. 6 Abs. 1 lit. a DSGVO. Der Zugriff auf Informationen in Ihrem Endgerät erfolgt auf Grundlage Ihrer Einwilligung nach § 25 Abs. 1 TDDDG.

Verarbeitete Daten können insbesondere IP-Adresse, Geräteinformationen, Nutzungsdaten, Referrer-URL, Interaktionen und ungefähre Standortdaten sein.

Weitere Informationen: https://policies.google.com/privacy?hl=de
```

### 2.3 Google Tag Manager

```text
Google Tag Manager

Wir verwenden den Google Tag Manager der Google Ireland Limited. Der Google Tag Manager verwaltet Website-Tags. Der Tag Manager selbst setzt nach Angaben von Google keine Cookies zur Nutzerprofilbildung, kann aber andere Tags auslösen, die ihrerseits Daten verarbeiten.

Nicht erforderliche Tags werden erst nach Ihrer Einwilligung ausgelöst.
```

### 2.4 PostHog

```text
PostHog

Wir verwenden PostHog zur Analyse der Nutzung unserer Website und zur Verbesserung unseres Angebots. PostHog wird nur eingesetzt, wenn hierfür eine Rechtsgrundlage besteht; bei nicht erforderlicher Analyse erfolgt der Einsatz nur nach Einwilligung.

Rechtsgrundlage ist Art. 6 Abs. 1 lit. a DSGVO, soweit eine Einwilligung eingeholt wird. Der Zugriff auf Informationen in Ihrem Endgerät erfolgt auf Grundlage von § 25 Abs. 1 TDDDG.

Weitere Informationen: https://posthog.com/privacy
```

### 2.5 Hotjar

```text
Hotjar

Wir verwenden Hotjar, um das Nutzungsverhalten auf unserer Website besser zu verstehen. Hotjar wird nur nach Ihrer Einwilligung geladen.

Rechtsgrundlage ist Art. 6 Abs. 1 lit. a DSGVO. Der Zugriff auf Informationen in Ihrem Endgerät erfolgt auf Grundlage von § 25 Abs. 1 TDDDG.

Weitere Informationen: https://www.hotjar.com/privacy/
```

### 2.6 Stripe

```text
Stripe

Für Zahlungsabwicklungen verwenden wir Stripe. Anbieter für Nutzer im Europäischen Wirtschaftsraum ist Stripe Payments Europe, Limited, 1 Grand Canal Street Lower, Grand Canal Dock, Dublin, Irland.

Zur Zahlungsabwicklung werden insbesondere Zahlungsdaten, Kontaktdaten, Transaktionsdaten und technische Daten verarbeitet. Rechtsgrundlage ist Art. 6 Abs. 1 lit. b DSGVO, soweit die Verarbeitung zur Vertragserfüllung erforderlich ist.

Weitere Informationen: https://stripe.com/de/privacy
```

### 2.7 Brevo

```text
Brevo

Für den Versand von E-Mails und Newslettern verwenden wir Brevo. Anbieter ist die Sendinblue GmbH, Köpenicker Straße 126, 10179 Berlin, Deutschland.

Newsletter werden nur mit Einwilligung versendet. Rechtsgrundlage ist Art. 6 Abs. 1 lit. a DSGVO. Sie können Ihre Einwilligung jederzeit mit Wirkung für die Zukunft widerrufen.

Weitere Informationen: https://www.brevo.com/de/legal/privacypolicy/
```

### 2.8 Supabase

```text
Supabase

Wir verwenden Supabase für Datenbank-, Authentifizierungs- oder Backend-Funktionen. Je nach Konfiguration können personenbezogene Daten wie Kontaktdaten, Accountdaten, Nutzungsdaten und technische Daten verarbeitet werden.

Rechtsgrundlage ist Art. 6 Abs. 1 lit. b DSGVO, soweit die Verarbeitung zur Bereitstellung unserer Dienste erforderlich ist, sowie Art. 6 Abs. 1 lit. f DSGVO für den sicheren und effizienten Betrieb.

Weitere Informationen: https://supabase.com/privacy
```

### 2.9 Cloudflare

```text
Cloudflare

Wir verwenden Cloudflare zur sicheren und performanten Bereitstellung unserer Website, insbesondere als Content Delivery Network, DNS- oder Sicherheitsdienst.

Verarbeitet werden können insbesondere IP-Adressen, technische Zugriffsdaten und Sicherheitsereignisse. Rechtsgrundlage ist Art. 6 Abs. 1 lit. f DSGVO. Unser berechtigtes Interesse liegt in Sicherheit, Verfügbarkeit und Performance der Website.

Weitere Informationen: https://www.cloudflare.com/privacypolicy/
```

### 2.10 YouTube / Vimeo Embeds

```text
Eingebettete Videos

Auf unserer Website können Videos von Drittanbietern eingebettet sein. Solche Inhalte werden erst geladen, wenn Sie dem Laden externer Medien zugestimmt oder den Inhalt aktiv aktiviert haben.

Beim Laden externer Videos können personenbezogene Daten, insbesondere IP-Adresse und Nutzungsdaten, an den jeweiligen Anbieter übermittelt werden.
```

---

## 3. Impressum — Section Templates

### 3.1 Angaben gemäß § 5 DDG

```text
Angaben gemäß § 5 Digitale-Dienste-Gesetz (DDG)

{{legalEntityName}}
{{streetAddress}}
{{postalCityCountry}}

Vertreten durch: {{authorizedRepresentative}}

Kontakt:
E-Mail: {{contactEmail}}
Website: {{websiteUrl}}
```

### 3.2 Register And VAT (Conditional)

```text
Registereintrag:
Registergericht: {{registerCourt}}
Registernummer: {{registerNumber}}

Umsatzsteuer-Identifikationsnummer gemäß § 27a Umsatzsteuergesetz:
{{vatId}}
```

### 3.3 Regulated Profession (Conditional)

```text
Berufsrechtliche Angaben

Zuständige Aufsichtsbehörde: {{supervisoryAuthority}}
Berufsbezeichnung: {{professionalTitle}}
Staat der Verleihung: {{professionalTitleCountry}}
Berufsrechtliche Regelungen: {{professionalRules}}
```

### 3.4 Editorial Responsibility (Conditional: hasEditorialContent)

```text
Verantwortlich für journalistisch-redaktionelle Inhalte gemäß § 18 Abs. 2 Medienstaatsvertrag (MStV):

{{editoriallyResponsiblePerson}}
{{editoriallyResponsibleAddress}}
```

### 3.5 Dispute Resolution — VSBG

```text
Verbraucherstreitbeilegung

Wir sind nicht bereit oder verpflichtet, an Streitbeilegungsverfahren vor einer Verbraucherschlichtungsstelle teilzunehmen.
```

If the business is willing or legally obliged to participate, replace with:

```text
Verbraucherstreitbeilegung

Wir sind bereit bzw. verpflichtet, an Streitbeilegungsverfahren vor einer Verbraucherschlichtungsstelle teilzunehmen.

Zuständige Verbraucherschlichtungsstelle:
{{consumerArbitrationBodyName}}
{{consumerArbitrationBodyAddress}}
Website: {{consumerArbitrationBodyWebsite}}
```

---

## 4. AGB — Section Templates

### 4.1 Geltungsbereich

```text
Geltungsbereich

Diese Allgemeinen Geschäftsbedingungen (AGB) gelten für die Nutzung von {{brandName}} unter {{websiteUrl}} sowie für die darüber angebotenen Leistungen.
```

### 4.2 Anbieter

```text
Anbieter

Anbieter von {{brandName}} ist:

{{legalEntityName}}
{{streetAddress}}
{{postalCityCountry}}
E-Mail: {{contactEmail}}

Vertreten durch: {{authorizedRepresentative}}
```

### 4.3 Vertragsgegenstand

```text
Vertragsgegenstand

{{brandName}} stellt digitale Leistungen bereit. Der konkrete Leistungsumfang ergibt sich aus der jeweiligen Leistungsbeschreibung auf der Website zum Zeitpunkt des Vertragsschlusses.
```

### 4.4 Vertragsschluss

```text
Vertragsschluss

Die Darstellung von Leistungen auf der Website stellt noch kein rechtlich bindendes Angebot dar. Ein Vertrag kommt erst zustande, wenn der Nutzer ein Angebot abgibt und dieses durch uns angenommen wird oder wenn der Bestellprozess dies ausdrücklich vorsieht.
```

### 4.5 Preise And Payment (Conditional: hasPaidFeatures)

```text
Preise und Zahlung

Es gelten die zum Zeitpunkt der Bestellung angegebenen Preise. Alle Preise verstehen sich, soweit gegenüber Verbrauchern angegeben, einschließlich der gesetzlichen Umsatzsteuer und sonstiger Preisbestandteile. Zusätzlich anfallende Kosten werden vor Abschluss der Bestellung angezeigt.

Die verfügbaren Zahlungsmethoden werden im Bestellprozess angezeigt.
```

### 4.6 Widerruf Reference (Conditional: hasB2C && hasPaidFeatures)

```text
Widerrufsrecht für Verbraucher

Verbrauchern steht bei Fernabsatzverträgen grundsätzlich ein gesetzliches Widerrufsrecht zu. Einzelheiten ergeben sich aus der Widerrufsbelehrung.
```

### 4.7 Haftung And Gewährleistung

```text
Gewährleistung und Haftung

Es gelten die gesetzlichen Gewährleistungsrechte. Für Schäden haften wir nach den gesetzlichen Vorschriften. Haftungsbeschränkungen gelten nicht bei Verletzung von Leben, Körper oder Gesundheit, bei Vorsatz oder grober Fahrlässigkeit sowie bei zwingender gesetzlicher Haftung.
```

### 4.8 Datenschutz

```text
Datenschutz

Informationen zur Verarbeitung personenbezogener Daten finden Sie in unserer Datenschutzerklärung unter {{websiteUrl}}/datenschutz.
```

### 4.9 Schlussbestimmungen

```text
Schlussbestimmungen

Es gilt das Recht der Bundesrepublik Deutschland unter Ausschluss des UN-Kaufrechts, soweit dem keine zwingenden Verbraucherschutzvorschriften entgegenstehen.
```

---

## 5. Widerrufsbelehrung — B2C Conditional Draft

```text
Widerrufsbelehrung

Verbraucher haben das Recht, binnen vierzehn Tagen ohne Angabe von Gründen diesen Vertrag zu widerrufen.

Die Widerrufsfrist beträgt vierzehn Tage ab dem Tag des Vertragsschlusses, soweit keine abweichende gesetzliche Regelung gilt.

Um Ihr Widerrufsrecht auszuüben, müssen Sie uns

{{legalEntityName}}
{{streetAddress}}
{{postalCityCountry}}
E-Mail: {{contactEmail}}

mittels einer eindeutigen Erklärung über Ihren Entschluss, diesen Vertrag zu widerrufen, informieren.

Zur Wahrung der Widerrufsfrist reicht es aus, dass Sie die Mitteilung über die Ausübung des Widerrufsrechts vor Ablauf der Widerrufsfrist absenden.

Folgen des Widerrufs:
Wenn Sie diesen Vertrag widerrufen, erstatten wir Ihnen alle Zahlungen, die wir von Ihnen erhalten haben, unverzüglich und spätestens binnen vierzehn Tagen ab dem Tag, an dem die Mitteilung über Ihren Widerruf dieses Vertrags bei uns eingegangen ist, soweit keine abweichende gesetzliche Regelung gilt.
```

### Model Withdrawal Form

```text
Muster-Widerrufsformular

Wenn Sie den Vertrag widerrufen wollen, füllen Sie bitte dieses Formular aus und senden Sie es zurück an:

{{legalEntityName}}
{{streetAddress}}
{{postalCityCountry}}
E-Mail: {{contactEmail}}

Hiermit widerrufe ich den von mir abgeschlossenen Vertrag über die Erbringung der folgenden Leistung:

Bestellt am:
Name des Verbrauchers:
Anschrift des Verbrauchers:
Datum:
Unterschrift des Verbrauchers (nur bei Mitteilung auf Papier):
```

---

## 6. Cookie-Hinweis Texts

### Banner

```text
Headline: Wir verwenden Cookies
Description: Wir verwenden Cookies und ähnliche Technologien. Einige sind erforderlich, damit diese Website funktioniert. Andere helfen uns, die Website zu analysieren und zu verbessern oder Marketinginhalte auszuspielen. Nicht erforderliche Cookies setzen wir nur mit Ihrer Einwilligung ein.
```

### Categories

```text
Essenziell:
Name: Essenzielle Cookies
Description: Diese Cookies sind für den Betrieb der Website erforderlich und können nicht deaktiviert werden.

Statistik:
Name: Statistik-Cookies
Description: Diese Cookies helfen uns zu verstehen, wie Besucher unsere Website nutzen. Sie werden nur mit Ihrer Einwilligung gesetzt.

Marketing:
Name: Marketing-Cookies
Description: Diese Cookies werden verwendet, um Werbung und Inhalte relevanter zu gestalten. Sie werden nur mit Ihrer Einwilligung gesetzt.
```

### Button Labels

```text
Alle akzeptieren: Alle akzeptieren
Alle ablehnen: Alle ablehnen
Nur essenzielle: Nur essenzielle
Auswahl bestätigen: Auswahl bestätigen
Cookie-Einstellungen: Cookie-Einstellungen
```

---

## Usage Notes

- All legal text uses formal "Sie".
- All legal references are German law and directly applicable EU law.
- Never use Austrian-specific laws or terms as governing rules: ECG, FAGG, MedienG, TKG 2021, BaFG, WKO, Firmenbuchgericht, UID-Nummer.
- Never add OS/ODR platform links; the EU platform was discontinued on 20 July 2025.
- Replace all placeholders with confirmed project-specific values.
- Conditional sections must only be included when the respective condition applies.
- This file is a drafting aid and does not replace legal review.
