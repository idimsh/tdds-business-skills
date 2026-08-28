---
name: transcreate
description: "Translate a document into another language by re-authoring it so it reads as if it had been written in that language first, not mapped word for word. Produces text a native reader would never suspect was translated, with structure, numbers and verifiable data preserved. TRIGGERS: 'transcreate', 'translate properly', 'not word for word', 'this reads like a translation', 'this sounds translated', 'localize this document', 'تعريب وليس ترجمة', 'make this sound native'. Also use when reviewing an existing translation that reads like a calque, or when creating the second language of a bilingual document pair. Works for any language pair in either direction."
---

# Transcreation

Translate by **re-authoring**, not by mapping words.

Read the source unit, understand what it does to its reader, then write that effect in the target language from scratch. The output is judged by one question, and only this one:

> If a native reader saw only the target text, would they suspect for a moment it was translated?

If yes, it is not finished. Rewrite it.

The discipline has several names, and each one names a different half of it. **Transcreation** is the localization industry's term: recreate the message so it lands the same, and let the words go free. **Domestication** is Lawrence Venuti's term from *The Translator's Invisibility* (1995): make the text read as natively written, the opposite of *foreignization*, which preserves the strangeness of the original. **Sense-for-sense** is the oldest, from Jerome's letter to Pammachius around 395 CE — *non verbum e verbo, sed sensum exprimere de sensu*, "to express not word from word, but sense from sense."

Arabic has a single word for it, **تعريب**, which literally means "to make it Arabic." Most languages do not: *Germanization* and *Japanization* mean cultural assimilation, not translation. That absence is worth noticing, because the missing word is exactly the thing agents get wrong. Word-for-word output is the default failure mode, and it is invisible to anyone who cannot read the target language — which is usually the person who requested the translation.

This skill is any language to any language. Nothing below assumes English on either side.

## Project Context

$ARGUMENTS

## Agent Portability

This skill must work across Codex, Claude, and other SKILL.md-compatible agents.

- Do not assume slash commands or a specific runtime.
- Read project instructions and context files when present: `AGENTS.md`, `CLAUDE.md`, `README.md`, style guides, existing glossaries, and files the user referenced. A project glossary, if one exists, outranks anything in this skill.
- Use the available search and file-read tools to find sibling documents in the same language pair, and match their established terminology rather than inventing your own.
- If parallel sub-agents are available, use them per document. Give each one the glossary and one finished exemplar to match. Do not split a single document across agents — register will drift at the seams.

## When To Use

Good targets:

- A document that must exist in two or more languages and say the same thing in each
- An existing translation that reads stiff, foreign, or machine-made
- Marketing, product, or narrative copy where the effect matters more than the wording
- Documentation whose readers are native speakers of the target language
- A bilingual pair that must stay checkable against each other over time

Do not use this skill for:

- Legal contracts, regulatory filings, medical dosing, or safety instructions where a certified human translator is required and literal fidelity is the legal standard
- Short UI strings with no surrounding context (they need a glossary and screenshots, not prose judgement)
- Quoted speech, testimony, or evidence, where changing the wording changes the meaning of the record
- Code identifiers, API field names, and config keys, which are not prose and must not be touched

## Two Entry Points

**A source with no target yet.** Work through the sections below in order.

**An existing target that reads translated, or a source that has moved ahead of its target.** Do not retranslate the whole file. A full rewrite discards ratified terminology and reopens decisions that were already settled, and it is far more work than the defect deserves. Recover the source version the target was built from where the history allows it and diff against that; otherwise read the pair section by section. Rewrite only what is wrong or stale, add every new decision to the glossary, then read the finished target end to end in one pass — patched documents drift at the seams, which is trap 5 below.

## Decide Before Writing A Single Word

Get these wrong and every sentence inherits the error. If the answer is not in the request and not inferable from the source, **ask**. Do not default silently.

1. **Which variety.** Arabic: Modern Standard, or a specific dialect? Chinese: Simplified or Traditional, and for which region? Portuguese: pt-BR or pt-PT? Spanish: Peninsular or Latin American? Norwegian: Bokmål or Nynorsk? "Translate to Arabic" is underspecified.
2. **Register and formality.** Many languages force a choice the source may have dodged: T–V distinction (tu/vous, du/Sie, tú/usted), Japanese plain vs です・ます vs 敬語, Korean speech levels. Pick once, state it, hold it for the whole document. Drifting mid-document is the loudest tell of machine output.
3. **Who the reader is.** A spec for engineers and a landing page for buyers get different vocabulary in the same language pair.
4. **What stays in the source script.** Product names, code identifiers, legal citations, and technical terms with no settled target equivalent. Decide the list up front.
5. **Which numeral system.** See Hard Constraints. This is a project decision, not a default.
6. **Where the target goes.** Default to a sibling of the source with a language tag in the name — `plan.md` becomes `plan.ar.md` — and never overwrite the source. If the project already pairs files some other way, follow that instead.

## The Five Traps

These are where transcreation fails. They are the same five in every language pair.

**1. The calque — inventing a target word for a term that has none.**

The worst failure, because the reader must decode it *and* it sounds wrong. A technical term with no settled equivalent either **stays in the source script** or is **described in ordinary language**. Never manufacture one.

Worked example: English `crawler` rendered into Arabic as `الزاحف`, literally "the crawling thing." No Arabic speaker says this. Correct options are to keep `crawler`, or to write "a tool that reads the site's pages."

**2. Source syntax wearing target words.**

Every clause is target vocabulary in source word order. Grammatically legal, unmistakably foreign. Verb-initial languages need verbs early; verb-final languages need them last; languages with free word order still have an unmarked one. Restructure the sentence rather than transliterating its shape.

**3. Idioms carried across.**

Replace the *effect*, never the image. "Move the needle" has no needle in it. "Low-hanging fruit" is not fruit. If the target has an idiom of equal register, use it; if not, say the plain thing. Idioms are the fastest way to sound translated.

**4. Labels translated instead of named.**

Headings, UI strings, column names, and section titles are **named** for what they do, not translated word for word.

Worked example: a section heading "The bet" rendered into Arabic as `الرهان` — literally a wager, as in gambling. What the section does is state the core of the idea, so it is named `جوهر الفكرة`.

**5. Register drift.**

Formality set in paragraph one and quietly abandoned by paragraph nine. Pick the level, hold it to the end.

## Hard Constraints

Break one of these and the output is wrong regardless of how well it reads.

- **Mirror the structure exactly.** Same headings at the same levels in the same order, same tables with the same rows and columns, same list and checkbox counts, same code blocks, same blockquotes. Only the language is re-authored.
- **Add nothing.** No explanations, framings, or editorial sentences that have no source counterpart. Adding content is not transcreation, and it breaks the pair silently: structural checks count sections, not sentences, so an invented paragraph passes every automated test.
- **Drop nothing.** A source sentence you find redundant still gets said.
- **Never alter verifiable data**: numbers, prices, dates, URLs, version strings, product names, legal citations, identifiers. These must stay checkable against the source.
- **Numerals are a project decision that must be stated, not assumed.** Ask if it is not specified. Western/ASCII digits are the safer default in any reference document, because they let a reader compare the pair at a glance and let a script diff the numbers; target-native digits (٤٢, ४२) suit prose written purely for native readers. Whichever is chosen, apply it to everything — prose, tables, prices, dates, percentages, scores. Note that the systems do not map one to one: Arabic `٫` is the decimal separator while `٬` is the thousands separator, and treating them as interchangeable corrupts values. Latin script has the same trap — English `1,000.5` is German `1.000,5`, and `3,141` is not `3141`. The **value** never changes either way.
- **Localize formats only in user-facing UI copy.** In a reference document where the pair must stay comparable, keep date and number formats identical.
- **Preserve markup and escapes.** A Markdown checkbox written `- [ ] 1\. text` needs the backslash in both files, or the list parses as a nested ordered list and the checkbox disappears.
- **Match the source's comment density and naming style** when transcreating code comments or documentation inside code.

## Script And Language Family Notes

| Family | What bites |
|---|---|
| RTL (Arabic, Hebrew, Farsi, Urdu) | Direction is a rendering concern, not a text one. Do not hand-insert bidi control characters. Mixed LTR runs such as URLs, code, and Latin product names need no markup in well-formed HTML with `dir` set. Prefer CSS logical properties over left/right. |
| CJK | No inter-word spaces; do not transplant source spacing. Measure words in Chinese, counters in Japanese. Line-breaking rules differ. Full-width vs half-width punctuation is a real choice. |
| Japanese, Korean | Politeness level is a decision, never a default. Honorifics encode the reader relationship, not the sentence. |
| German | Compounds instead of noun phrases. Sie or du must be chosen. Sentence-final verbs restructure long sentences. |
| Romance | T–V distinction; gender agreement cascades through adjectives and past participles. |
| Slavic | Cases and verbal aspect mean a "literal" verb choice is often simply wrong. |
| Turkish, Finnish, Hungarian | Agglutination: a source noun phrase often becomes a single word. |

## Build The Glossary Once, Then It Is Binding

For any document set, keep a terminology file beside the documents. Three columns minimum:

```markdown
| Source term | Target | Never |
|---|---|---|
| crawler | أداة تقرأ صفحات الموقع، or `crawler` | ❌ الزاحف |
| The bet | جوهر الفكرة | ❌ الرهان |
| backend | الأنظمة الخلفية، or `backend` | ❌ الواجهة الخلفية |
```

The **Never** column is what makes it work. It records rejected renderings so the same mistake cannot return after the reasoning behind it is forgotten. Add a term to the glossary *before* using it in a document.

A grep over the Never column then becomes a regression test:

```bash
grep -rl 'الزاحف\|الرهان\|الواجهة الخلفية' path/to/*.ar.md   # must return nothing
```

Keep banned terms out of your own changelog and log entries, or the guard reports itself and stops being trusted.

## Verify Before Reporting Done

Run the structural check in `parity-check.md`, which compares headings per level, tables, checkboxes, code blocks, blockquotes, list items, and the URL set between the two files. It exits non-zero on divergence and needs only Python 3. Write it to a scratch path outside the document tree — it is a disposable tool, not a project file.

**Comparing numbers across languages is harder than it looks.** Symbols sit on different sides (`€500–1,500` against `500–1,500 €`) and separators swap roles (`1,000.5` against `1.000,5`, and Arabic `٬` against `٫`), so a naive extractor mismatches every price in the document. The script handles both; anything you write yourself must too, and must never strip a decimal separator as though it were a grouping mark. What no extractor can handle is a number spelled out in words (`مليون دولار` for `$1,000,000`), which is correct, not a defect. Treat number warnings as *look here*, never as *fix this*.

Then the checks no script can do:

- Read the target **aloud in your head, without looking at the source.** Every sentence that makes you pause is a sentence to rewrite.
- Grep the glossary's Never column.
- Spot-check three numbers and every URL against the source by eye.

**Structural parity is a floor, not proof.** A pair can pass every automated check and still be wrong, and stale content passes forever, because the check counts shape rather than currency. When the source contradicts itself — a step describing work its own status section says is finished, a reference to something deleted — that is a source defect surfaced by careful reading. Fix it in **both** languages and say what you changed. Do not mirror a known defect faithfully into a second language.

## Report Back

- Files written, and the parity result for each.
- **Every term you had to decide on that was not already in the glossary**, so it can be ratified and added. This is the most valuable part of the report.
- The register and variety you chose, if they were not specified.
- Anything in the source that looked stale, self-contradictory, or wrong.

## Rules

- Re-author, never map. The test is whether a native reader would suspect translation.
- Never invent a target word for a term that has none. Keep the source term or describe it.
- Same structure, same numbers, same URLs. Nothing added, nothing dropped.
- Hold one register for the whole document.
- The project glossary outranks this skill. Add to it before using a new term.
- Automated parity is a floor. Reading it aloud is the real check.
