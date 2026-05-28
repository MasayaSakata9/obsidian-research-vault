---
source: hermes-verifier
source_file: /home/sakata/.hermes/cron/output/b220c0ffee02/2026-05-28_23-29-30.md
sha256: 273fe3a253b47c6dc8846b3dcfc2309577067a05ae4b2c3baf394291cf0b1853
---

## Cron Job Audit Report: paper-reader-team (a110c0ffee01)

### Root Cause

The job was blocked by the prompt-injection scanner because the assembled prompt contained **invisible Unicode character U+200B (Zero-Width Space)**. This character was present in the `web_excerpt.excerpt` field of the cached source context file, which was fetched from `https://www.clawrxiv.io/abs/2603.00351`.

The invisible Unicode originated from the external web page content — clawRxiv embeds zero-width spaces in its HTML, which survived the HTML-to-text conversion and ended up in the script output that gets assembled into the cron job prompt.

### Fix Applied

**1. Added invisible Unicode sanitization to the script** (`~/.hermes/scripts/paper_review_selector.py`):

- `strip_invisible_unicode(text)` — removes U+200B, U+200C, U+200D, U+2060, U+FEFF, U+180E, bidi controls (U+202A–U+202F), and other invisible/formatting Unicode characters
- `sanitize_dict(obj)` — recursively strips invisible Unicode from all string values in nested dicts/lists

**2. Sanitization applied at three points:**

| Location | Line | Purpose |
|---|---|---|
| `html_to_text()` | 332 | Strips invisible Unicode during HTML-to-text conversion |
| `save_latest_source_context()` | 403 | Sanitizes cached JSON before writing to disk |
| `print(json.dumps(...))` | 445 | Sanitizes final script output (the prompt payload) |

**3. Cleaned the contaminated cache file:**

- `/home/sakata/.hermes/logs/latest-paper-source-context.json` was rewritten with clean content

### Verification

- The `paper-review-team` job (b220c0ffee02) uses an inline prompt template in `jobs.json` — no external skill files involved
- The `paper-reader-team` job (a110c0ffee01) also uses an inline prompt — no skill files attached
- The candidates file (`High_Priority_Paper_Candidates.md`) was checked and contains no invisible Unicode
- The threat pattern `_CRON_INVISIBLE_CHARS` in `cronjob_tools.py` blocks: U+200B, U+200C, U+200D, U+2060, U+FEFF, U+202A–U+202E — all of which are now stripped by the sanitization functions

### Next Run

The next scheduled run at `2026-05-29T00:07:00+09:00` should proceed normally. The script will fetch fresh web data, sanitize it, and output clean JSON without invisible Unicode characters.
