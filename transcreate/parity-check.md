# Parity Check — Structural Comparison Of A Transcreated Pair

Reference file for the `transcreate` skill. Contains a portable Python 3 script that verifies a target document mirrors its source structurally, plus the manual checks the script cannot perform.

**What it checks:** headings per level, tables, checkboxes, fenced code blocks, blockquotes, list items, and the set of URLs. Optionally, with `--numbers`, it compares number multisets, folding target-native digits to ASCII and normalising both files' grouping and decimal separators the same way.

**What it cannot check:** meaning, accuracy, currency, or register. A pair can pass every check here and still be a bad translation of stale content. Treat a pass as a floor, never as proof.

**Requirements:** Python 3, standard library only. No network, no packages.

## Usage

Write the script below to a working file, then run it:

```bash
python3 parity_check.py <source-file> <target-file> [--numbers] [--quiet]
```

Exit codes: `0` pass (possibly with warnings), `1` structural divergence, `2` bad arguments or an unreadable file.

To check a whole directory of pairs, loop over the sources and derive each target path from your project's naming convention.

## The Script

```python
#!/usr/bin/env python3
"""Structural parity check for a transcreated document pair.

Usage:  parity_check.py <source> <target> [--numbers] [--quiet]

Checks that the target mirrors the source's STRUCTURE. It cannot check meaning
or currency -- a file can pass every check here and still be stale or wrong.
"""
import re, sys, unicodedata
from collections import Counter

MUST = "must"; WARN = "warn"

def blocks(lines, pred):
    """Count contiguous runs of lines satisfying pred."""
    n, inside = 0, False
    for ln in lines:
        if pred(ln):
            if not inside: n += 1; inside = True
        else:
            inside = False
    return n

def strip_fences(lines):
    """Return lines outside fenced code blocks, plus the fence count."""
    out, fences, inside = [], 0, False
    for ln in lines:
        if re.match(r'^\s*(`{3}|~{3})', ln):
            fences += 1; inside = not inside; continue
        if not inside: out.append(ln)
    return out, fences

# Group separators: apostrophes (Swiss) and the various non-breaking spaces.
SEPS_RE = re.compile('[\u2019\'\u202f\u00a0\u2009\u2007 ]')
NUM_RE  = re.compile('\\d(?:[\\d.,\u066b\u066c\u2019\'\u202f\u00a0\u2009\u2007 ]*\\d)?')  # \u066b/\u066c = Arabic decimal/thousands

def norm_digits(s):
    """Fold Arabic-Indic, Persian, Devanagari, full-width etc. digits to ASCII.
    decimal(c, None) matters: the superscript in 'm2' as 'm\N{SUPERSCRIPT TWO}' is isdigit()
    but has no decimal value, and decimal(c) would raise ValueError on it."""
    out = []
    for c in s:
        d = unicodedata.decimal(c, None) if not c.isascii() else None
        out.append(str(d) if d is not None else c)
    return ''.join(out)

def fmt(v):
    return str(int(v)) if v == int(v) else repr(v)

def canon(tok):
    """Canonicalise one numeric token so the two files are comparable.

    '.' and ',' swap roles between languages (en 1,000.5 / de 1.000,5) and the
    two cannot always be told apart. The rule below resolves each token the SAME
    way on both sides, so an ambiguous pair (1,000 / 1.000, or 3.141 / 3,141)
    compares equal even where the guess is wrong. Consistency, not correctness,
    is what a parity check needs.
    """
    t = SEPS_RE.sub('', tok)
    t = t.replace('\u066c', ',').replace('\u066b', '.')   # Arabic thousands mark, decimal mark
    parts = re.split(r'[.,]', t)
    if len(parts) == 1:
        return fmt(float(t))
    if all(len(p) == 3 for p in parts[1:]) and 1 <= len(parts[0]) <= 3:
        return fmt(float(''.join(parts)))                   # every group is 3 digits -> grouping
    return fmt(float(''.join(parts[:-1]) + '.' + parts[-1]))  # last separator is the decimal one

def numbers(text):
    return Counter(canon(m.group()) for m in NUM_RE.finditer(norm_digits(text)))

def profile(path):
    try:
        raw = open(path, encoding='utf-8').read()
    except OSError as e:
        print(f"cannot read {path}: {e}"); sys.exit(2)
    lines = raw.splitlines()
    body, fences = strip_fences(lines)
    p = {}
    for lvl in (1, 2, 3, 4):
        p[f'h{lvl}'] = (sum(1 for l in body if re.match(r'^#{%d}\s' % lvl, l)), MUST)
    p['tables']      = (sum(1 for l in body if re.match(r'^\s*\|[\s:|-]*-[\s:|-]*\|', l)), MUST)
    p['checkboxes']  = (len(re.findall(r'^\s*[-*+]\s+\[[ xX]\]', '\n'.join(body), re.M)), MUST)
    p['code_blocks'] = (fences // 2, MUST)
    p['quotes']      = (blocks(body, lambda l: l.lstrip().startswith('>')), MUST)
    p['list_items']  = (sum(1 for l in body if re.match(r'^\s*([-*+]|\d+[.)])\s', l)), WARN)
    p['_urls']       = set(re.findall(r'https?://[^\s)>\]"\'`]+', raw))
    p['_numbers']    = numbers(raw)
    return p

def main():
    args = [a for a in sys.argv[1:] if not a.startswith('--')]
    flags = {a for a in sys.argv[1:] if a.startswith('--')}
    if len(args) != 2:
        print(__doc__); sys.exit(2)
    src, tgt = args
    a, b = profile(src), profile(tgt)

    rows, failed, warned = [], False, False
    for k in [k for k in a if not k.startswith('_')]:
        av, sev = a[k]; bv, _ = b[k]
        if av == bv:
            rows.append(("ok  ", k, av, bv))
        elif sev == MUST:
            failed = True; rows.append(("FAIL", k, av, bv))
        else:
            warned = True; rows.append(("warn", k, av, bv))

    ua, ub = a['_urls'], b['_urls']
    if ua == ub:
        rows.append(("ok  ", "urls", len(ua), len(ub)))
    else:
        failed = True; rows.append(("FAIL", "urls", len(ua), len(ub)))

    if '--quiet' not in flags:
        print(f"{'':4}  {'check':<12} {'source':>8} {'target':>8}")
        for st, k, av, bv in rows:
            print(f"{st}  {k:<12} {av:>8} {bv:>8}")

    if ua != ub:
        for u in sorted(ua - ub): print(f"      missing in target: {u}")
        for u in sorted(ub - ua): print(f"      extra in target:   {u}")

    if '--numbers' in flags:
        na, nb = a['_numbers'], b['_numbers']
        diff = [(n, na[n], nb[n]) for n in sorted(set(na) | set(nb), key=float) if na[n] != nb[n]]
        if diff:
            warned = True
            print("\n  numbers differing in count (spelled-out numerals cause false positives):")
            for n, x, y in diff: print(f"      {n:<12} source x{x}  target x{y}")
        else:
            print("\n  numbers: identical multisets")

    print()
    if failed:  print("RESULT: FAIL - structure diverges; fix before reporting done."); sys.exit(1)
    if warned:  print("RESULT: PASS with warnings - review them, they are often real."); sys.exit(0)
    print("RESULT: PASS - structure mirrors. This says nothing about meaning or currency.")

main()
```

## Reading The Output

| Row | Severity | Meaning |
|---|---|---|
| `h1`–`h4` | must match | A heading was added, dropped, or moved to another level |
| `tables` | must match | A table was dropped or split |
| `checkboxes` | must match | A task item vanished, often because `1.` was not escaped as `1\.` |
| `code_blocks` | must match | An unclosed fence, or a block dropped |
| `quotes` | must match | A blockquote was dropped or merged into the body |
| `list_items` | warning | Often a legitimate wrapping difference; sometimes a merged bullet |
| `urls` | must match | Prints exactly which URLs differ. A changed URL is always a defect |
| `numbers` | warning | Only with `--numbers`. Read the caveats below before acting |

## Number Warnings Are Not Defects

Three notation differences look like mismatches and are not. Two of them the script absorbs; one it cannot.

**Symbol placement — handled.** Currency and unit symbols sit on different sides in different languages: English writes `€500–1,500`, Arabic writes `500–1,500 €`. The extractor reads bare digit runs and ignores everything around them, so both endpoints of a range are collected from either file and the symbol side never matters. Anything you write yourself must do the same, or every price reports as a mismatch.

**Separator conventions — handled.** `.` and `,` swap roles across languages: `1,000.5` in English is `1.000,5` in German, and Arabic writes `٬` for thousands and `٫` for decimals. Some tokens are genuinely ambiguous — `1.000` and `1,000` could each be one thousand or a three-decimal fraction. `canon()` resolves every token by the same rule on both sides, so an ambiguous pair still compares equal even where the guess is wrong: a parity check needs consistency, not correctness. Never strip a decimal separator as if it were a grouping mark — that silently turns `3,141` into `3141`.

**Spelled-out numerals — not handled.** A number written in words in one language (`مليون دولار` for `$1,000,000`, `تسعين ثانية` for `90-second`) is idiomatic, not an error. The value is intact, only the notation differs, and no extractor will see it. This is the warning you will actually get, and it is usually fine.

Investigate a number warning by reading both lines. Fix only when the underlying **value** differs.

## Manual Checks The Script Cannot Do

- Read the target aloud in your head, without looking at the source. Every sentence that makes you pause is a sentence to rewrite.
- Grep the project glossary's **Never** column; it must return nothing.
- Spot-check three numbers and every URL against the source by eye.
- Confirm one register is held from the first paragraph to the last.
