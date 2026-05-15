# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**KNA** is the corrections repository for the Cologne digitization of Knauer's *Sanskrit-Russian Vocabulary*. The canonical source lives in `csl-orig/v02/kna/kna.txt`.

Issues and corrections for this dictionary are tracked in the [GitHub issue tracker](https://github.com/sanskrit-lexicon/KNA/issues).

## Common Commands

### Apply line-level corrections (standard pattern)
```bash
python updateByLine.py <input_file> <changein_file> <output_file>
```

### Rebuild and validate XML (from `csl-pywork/v02/`)
```bash
sh generate_dict.sh kna ../../KNAScan/2020
sh xmlchk_xampp.sh kna
```

## Dependencies

- **Python 3**
- **kna.txt** — in `$BASE/cologne/csl-orig/v02/kna/kna.txt`
