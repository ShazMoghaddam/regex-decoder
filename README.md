# Regular Expressions, Decoded

An interactive regex decoder. Paste a pattern and it explains every token in plain English, then lets you test it against your own text with the matches highlighted live. One self-contained HTML page, no build step, no dependencies.

**Live:** https://shazmoghaddam.github.io/regex-decoder/

## What it does

- Breaks a regex down token by token, colour-coded and indented by group depth, with a plain-English description of each part
- Tests the pattern against your own text and highlights every match as you type, using the browser's real regex engine
- Shows the capture groups (numbered and named) for the first match
- Handles a `/pattern/flags` input and reports which flags are active
- Validates with the native engine, so invalid patterns show the real error (for example "Unterminated group")
- Clickable presets (dates, email, URL, hex colour, IPv4, word boundary, named groups, postcode) that load a pattern and matching sample text
- Shareable URL that carries the pattern, flags and test text, plus copy buttons

## How it works

The whole thing is vanilla JavaScript in a single file. Two halves, on purpose:

The **explanation** is a hand-written tokenizer that walks the pattern and describes each construct: literals, character classes, sets and ranges, quantifiers (greedy and lazy), groups (capturing, non-capturing, named, lookaround), anchors, alternation and backreferences. It is best-effort by nature, so it is designed never to throw and to always account for the whole pattern.

The **matching** is not hand-written at all: it runs your pattern through the browser's native `RegExp`. So while the explanation is a guide, the highlighted matches and captured groups are exactly what a real engine produces. Zero-length matches are handled so scanning never hangs.

The tokenizer and scanner are covered by unit tests (full-coverage reconstruction of many patterns, edge cases like `[]]` and dangling escapes, group-depth tracking, and match behaviour including zero-length matches).

## Note on flavours

Matching uses JavaScript's regex engine, so it follows JS semantics. Most everyday patterns behave the same across languages, but engine-specific features (some lookbehind support, certain Unicode property escapes, POSIX classes) can differ from PCRE, Python's `re`, or others. The explanation covers the common constructs shared across flavours.

## Put it live on GitHub Pages

1. Create a new public repo named `regex-decoder`.
2. Add `index.html` and `preview.png` to the root of the repo.
3. Settings, then Pages. Source: "Deploy from a branch", branch `main`, folder `/ (root)`, Save.
4. Give it a minute, then open https://shazmoghaddam.github.io/regex-decoder/

If you name the repo something else, update the three absolute URLs near the top of `index.html` (`canonical`, `og:url`, `og:image`).

## Files

- `index.html` — the page (and the whole app)
- `preview.png` — the 1200x630 card shown when the link is shared

Built by Shaz Moghaddam.
