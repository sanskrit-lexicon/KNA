# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**KNA** is the corrections repository for the Cologne digitization of Knauer's *Sanskrit-Russian Vocabulary* (Friedrich Knauer, 1908 — part of *Учебник санскритского языка / Sanskrit Manual*). It is part of the [Cologne Digital Sanskrit Lexicons](https://www.sanskrit-lexicon.uni-koeln.de/) project.

The canonical source file lives in the sibling repo `csl-orig`:
```
../cologne/csl-orig/v02/kna/kna.txt
```

The dictionary contains 3,271 L-entries across 95 pages. It is a Sanskrit-Russian vocabulary.

## Architecture

| Directory / File | Purpose |
|---|---|
| `CLAUDE.md` | This file — developer documentation |
| `README.md` | Project overview for human readers |
| `CITATION.cff` | Academic citation metadata (cff 1.2.0) |

The repository currently holds only metadata and documentation. Source files are in `csl-orig`; build scripts live in `csl-pywork`.

## Key Commands

### Apply line-level corrections (standard Cologne pattern)
```bash
python updateByLine.py <input_file> <changefile> <output_file>
```

Change file format — paired lines, `;` prefix for comments:
```
1234 old original line text here
1234 new replacement line text here
```

### Rebuild and validate XML (from `csl-pywork/v02/`)
```bash
sh generate_dict.sh kna ../../KNAScan/2020
sh xmlchk_xampp.sh kna
```

## Dependencies

- **Python 3** with `sys.stdout.reconfigure(encoding='utf-8')` on Windows
- **kna.txt** — in `../cologne/csl-orig/v02/kna/kna.txt`
- **csl-pywork** — build and validation scripts

## Data Format

Every entry in `kna.txt` follows the Cologne markup convention:

| Tag | Role | Example |
|---|---|---|
| `<L>NNNN` | Entry begin with print line number | `<L>1` |
| `<LEND>` | Entry end | |
| `<k1>headword` | Primary headword in SLP1 | `<k1>aMSa` |
| `<k2>variant` | Secondary spelling | `<k2>aMSa` |
| `<lex>code` | Lexical category | `<lex>m.</lex>` |
| `<ls>source` | Literary source citation | `<ls>Rv.</ls>` |
| `<ab>tag` | Italicised abbreviation | `<ab>m.</ab>` |
| `{#text#}` | Sanskrit text in SLP1 transliteration | `{#aMSaH#}` |
| `{%text%}` | Italicised display text | `{%cf.%}` |

### Annotated Example Entry

```
<L>1<pc>001,1<k1>a<k2>a<e>1
{#a#} — prefix of negation or privation (like Gr. {%α, αν%}),
corresponding to Goth. {%un%}, Engl. {%un-, in-%} etc.
<LEND>
```
- `<L>1` — entry 1, page 001, column 1
- `<k1>a`, `<k2>a` — headword "a" in SLP1 (Devanagari: अ)
- `<e>1` — etymology marker
- `{#a#}` — Sanskrit text in SLP1
- `{%α, αν%}` — italicised Greek comparison
- `<LEND>` — entry close

## Encoding

- UTF-8 NFC throughout.
- Sanskrit text in SLP1 transliteration, wrapped in `{#…#}`.
- Display layer uses IAST (ISO 15919) and Devanagari, generated via `transcoder/`.
- Source language of definitions: Russian.
- Round-trip SLP1 ↔ IAST ↔ Devanagari is lossless for all correctly marked entries; exceptions are tracked with the `encoding` issue label.

## GitHub Issue Conventions

All issues follow the [Sanskrit Lexicon issue taxonomy](https://github.com/sanskrit-lexicon/KNA/issues).

### Milestones

| # | Milestone | Types |
|---|---|---|
| 1 | Dictionary to Book | `link-target`, `link-splitting` |
| 2 | Digitization Quality | `scan-quality`, `encoding`, `bug`, `text-correction` |
| 3 | Structured Data | `markup`, `question` |
| 4 | Major Enhancements | `content-enhancement` |

### Type labels (color `#0075ca`)

| Label | When to use |
|---|---|
| `link-target` | Building click-throughs from `<ls>` abbreviations to scanned PDF pages |
| `link-splitting` | Splitting combined `SOURCE N,N` refs into individual per-page links |
| `markup` | Normalising XML tag content |
| `text-correction` | Corrections to Russian definitions or Sanskrit headwords |
| `content-enhancement` | New material, display upgrades, structural additions |
| `encoding` | SLP1/AS/IAST transcoding, character rendering |
| `scan-quality` | Replacing blurry, skewed, or missing scan pages |
| `bug` | Broken links, XML errors, broken download files |
| `question` | Scholarly questions requiring research |

### Severity labels

| Label | Color | When to use |
|---|---|---|
| `minor` | `#e4e669` | Targeted, self-contained fix |
| `medium` | `#fbca04` | Standard unit of work |
| `hard` | `#d93f0b` | Large effort spanning many sources or files |
