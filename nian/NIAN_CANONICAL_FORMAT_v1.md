---
title: NIAN Canonical Format · v1.2.1
author: 釋慧鏡 (Shi Huijing) · drafted with Chat Opus
date: 2026-05-04
type: canonical_format_specification
status: locked
applies_to: four-practice-gates / nian / papers / part{1..8}_*/ (all 62 files)
supersedes_drift: NIAN_FORMAT_AUDIT_2026Q2 (2026-05-04, Code Sonnet 4.6)
parser_contract: aligns with waken/lib/papers.ts header expectations (see §5)
---

# NIAN Canonical Format · v1.2.1

**Purpose**: Single normative specification for the format of NIAN papers (`Mindful of the Buddha`, Volume 1 of *The Four Practice Gates*). This document locks the conventions revealed by the 2026-05-04 audit and provides the contract for: (a) the upcoming format-remediation pass against all 62 files, (b) the SĪLA / DĀNA / DHYĀNA volumes' format from first paper, (c) the future Waken site parser's ingestion of these files, (d) `PAPER_STYLE.md v2.2` of the four-practice-gates repo (which inherits from this).

**Scope**: format only. This spec does not touch content, doctrinal claims, citations, or the substantive shape of any paper. It locks landmarks and conventions.

**Authority**: Released under the four-practice-gates repo CC BY-NC-SA 4.0 license. Format decisions are author-locked (Max + Chat Opus 4.7 review, 2026-05-04). Decisions hold until explicitly superseded by `v2.0` or higher.

---

## 0. Quick reference — the 10 locked decisions

| # | Decision | Locked value |
|---|---|---|
| 1 | YAML quoting | All string values that contain `*`, `:`, `[`, `]`, `{`, `}`, `,`, `&`, `!`, `?`, `\|`, `>`, `#`, `@`, `` ` `` MUST be double-quoted. Default to double-quoting all string values for safety. |
| 2 | `paper_id` separator | Hyphen: `NIAN-P##` where `##` is two-digit zero-padded (`P01`–`P31`) |
| 3 | YAML date format | Full ISO `"YYYY-MM-DD"` (quoted) |
| 4 | YAML `version` field | Always present and quoted: `"1.0"` for shipped papers; semver-ish increment for revisions |
| 5 | YAML `series` field | EN: `"Four Practice Gates, Volume 1 — Mindful of the Buddha: From Early Buddhist Recollection to the Flower Ornament Vision"` · ZH: `"四行門卷一《念——從阿含隨念到華嚴念佛》"` (always quoted) |
| 6 | Section numbering | EN top-level (`## `): Roman (`I.`, `II.`, …) · EN sub-section (`### `): Arabic decimal (`2.1`, `3.4`) · ZH top-level: 中文數字 (`一、`, `二、`) · ZH sub-section: Arabic decimal (same as EN) |
| 7 | EN punctuation Type A — transliteration parens | Half-width `( ... )` when content is romanized or mixed; full-width `（ ... ）` only when content is pure-CJK as a CJK insertion |
| 8 | EN punctuation Type B — Chinese-source quotes | Keep `「...」` intact when the content is an intentional citation of a Chinese-language term or phrase |
| 9 | Footer block | Three italic paragraphs, in this order: (i) optional CBETA collation notice, (ii) repo + license line, (iii) Form B attribution: `*NIAN-P## · 釋慧鏡 (Shi Huijing) · YYYY-MM-DD*` (EN) or `*NIAN-P## · 釋慧鏡 · YYYY-MM-DD*` (ZH) |
| 10 | References section | EN heading `## References` (kill `Bibliography`) · ZH heading `## 參考文獻` (restore where missing) · 3-part Roman sub-headings as default; 5-part expansion only when content warrants |
| 11 | Title-block second bold line | Permitted for **Coda papers (P30/P31)** AND **Part-opener papers (P01, P05, P06, P09, P12, P20, P25)**; first bold line under H1 is the subtitle (parser-extracted), second bold line is the structural-position marker |
| 12 | CBETA collation notice | **Required** when paper cites ≥1 T/X/A number; canonical italic template per §2.6 |

---

## 1. YAML frontmatter contract

### 1.1 Required fields (all papers)

```yaml
---
title: "<Full title with subtitle>"
author: "釋慧鏡 (Shi Huijing)"
series: "Four Practice Gates, Volume 1 — Mindful of the Buddha: From Early Buddhist Recollection to the Flower Ornament Vision"
paper_id: "NIAN-P01"
date: "2026-04-14"
version: "1.0"
license: "CC BY-NC-SA 4.0"
---
```

ZH file uses identical structure with ZH-canonical `series:`:

```yaml
---
title: "<Chinese full title>"
author: "釋慧鏡 (Shi Huijing)"
series: "四行門卷一《念——從阿含隨念到華嚴念佛》"
paper_id: "NIAN-P01"
date: "2026-04-14"
version: "1.0"
license: "CC BY-NC-SA 4.0"
---
```

### 1.2 Quoting policy

**Every string value is double-quoted.** This is non-negotiable and resolves the audit's Anomaly #9 (the bare-asterisk YAML alias bug in P04/P06/P07/P08 EN).

Rationale: pyyaml and most standard YAML parsers treat `*Mindful` as an alias reference (since `*` is a YAML control character). The four affected EN files have technically-invalid YAML that happens to render fine in markdown viewers but breaks any programmatic ingestion. Double-quoting all string values eliminates the entire class of issues for `*`, `:`, `&`, `[`, `{` and similar characters.

✗ **Wrong**: `series: *Mindful of the Buddha*, Volume I of *The Four Practice Gates*`
✓ **Right**: `series: "Four Practice Gates, Volume 1 — Mindful of the Buddha: From Early Buddhist Recollection to the Flower Ornament Vision"`

(Note: the canonical `series:` value also drops the markdown italics from the source-file YAML. Italics in YAML strings are confusing and serve no display purpose since YAML is metadata, not body content.)

### 1.3 `paper_id` (Decision #2)

Format: `NIAN-P##` — hyphen, capital P, two-digit zero-padded number.

Regex contract: `^NIAN-P(0[1-9]|[12]\d|3[01])$`

Audit found 10 files using `NIAN_P##` (underscore). Underscore is invalid; remediate to hyphen.

### 1.4 `date` field (Decision #3)

Format: full ISO `"YYYY-MM-DD"`, quoted.

For the 16 files where the `date:` field is absent (P01–P08 EN/ZH), reconstruct from the git first-commit date of that file. For files where only `"YYYY-MM"` (month-precision) is present, reconstruct day-precision from git first-commit; if unavailable, use day `15` as conservative middle-of-month fallback and document the reconstruction in the remediation commit message.

### 1.5 `version` field (Decision #4)

Format: quoted string, semver-ish.

- `"1.0"` for first shipped state
- `"1.1"` for minor corrections (typo fixes, broken-link repairs, citation polishing)
- `"2.0"` for substantive revision (added section, retracted argument, structural rewrite)

P01–P08 are missing this field; remediate to `"1.0"`. Files marked as revisions in their content carry whatever version they should — do not retroactively renumber.

### 1.6 `series` field (Decision #5)

Single canonical value per language. The audit found 11+ ZH variants and 5+ EN variants — all converge to:

- EN: `"Four Practice Gates, Volume 1 — Mindful of the Buddha: From Early Buddhist Recollection to the Flower Ornament Vision"`
- ZH: `"四行門卷一《念——從阿含隨念到華嚴念佛》"`

This is the canonical book title (per `FOUR_GATES_MASTER_PLAN.md` §1.1 EN/ZH naming + §2.1 卷一 ZH naming). Both must be quoted to handle the colon + em-dashes safely.

For SĪLA / DĀNA / DHYĀNA, the `series:` value increments by volume number and replaces the colon-after-dash subtitle clause with that volume's settled subtitle.

### 1.7 ZH-only optional YAML fields

ZH files may include the following optional YAML fields (recognized as legitimate metadata, not drift):

```yaml
part: 7              # 1–8, matches the volume part number
sequence: 28         # 1–31, matches paper number within volume (== paper_id minus prefix)
language: zh-Hant    # ISO 639-1 + ISO 15924 subtag for Traditional Chinese
```

These fields are present in some ZH files (P27, P28, P29, P31 ZH per the 2026-05-04 audit) and absent in others. Both states are canonical. The Waken parser ignores unknown YAML fields gracefully, so these fields do not affect parsing. Phase C remediation must preserve these fields where they exist; do not add them where they don't.

EN files do not carry these fields; the volume-and-sequence relationship for EN files is derived from `paper_id` directly.

---

## 2. Document structure

### 2.1 Top of document — H1, subtitle, bilingual subtitle

```markdown
# <H1 — primary title only, no subtitle>

**<Bold subtitle line — single line, may break with em-dash>**

*中文主題：<ZH counterpart of subtitle>*

---
```

Notes:
- H1 is the primary title only (e.g. `# Holding the Name`); the subtitle does not appear inside H1
- Subtitle is one line, bolded
- 中文主題 line uses **full-width colon** `：` (the content is CJK, so the punctuation should be too) — Decision derived from #7
- A horizontal rule `---` closes this block

The audit found 3 EN files using half-width colon (`:`) here; remediate to full-width.

ZH files do not have the `*中文主題：…*` line (it would be redundant). ZH files use the same H1 + bold-subtitle structure but in Chinese only.

### 2.2 Author metadata block

After the first `---`, before the body proper:

```markdown
**Author:** 釋慧鏡 (Shi Huijing)
**Date:** April 2026
**License:** CC BY-NC-SA 4.0
**Version:** 1.0
**Series:** NIAN — Mindful of the Buddha

---
```

Notes:
- Block appears in **both EN and ZH files** (audit found ZH files lack this; remediate to add)
- `**Date:**` here is human-readable (`April 2026`, `May 2026`); the precise YYYY-MM-DD lives in YAML only
- `**Series:**` is the short form (`NIAN — Mindful of the Buddha`), not the full canonical YAML series string
- Block closes with a second `---`

P01–P08 EN/ZH lack this block; remediate to add. The author-block-presence pattern (P09+ has it) was a model-cohort artifact, not a deliberate convention split — collapse to "always present."

### 2.3 Abstract and Keywords

EN file:

```markdown
## Abstract

<one paragraph, 30–1200 words per Waken paper-schema contract>

**Keywords:** <comma-separated terms>

---
```

ZH file:

```markdown
## 摘要

<一段，50–1500 漢字>

**關鍵詞：** <頓號分隔之術語>

---
```

Notes:
- **One language per file**: EN file has `## Abstract` only; ZH file has `## 摘要` only. The 13 EN files currently with bilingual `## Abstract` + `## 摘要` stacking get the ZH heading removed. (The bilingual stacking was redundant — readers go to the ZH file for Chinese content.)
- `**Keywords:**` (EN) and `**關鍵詞：**` (ZH, with full-width colon) are bolded inline, not headed
- Block closes with a third `---`

### 2.4 Body section numbering (Decision #6)

EN top-level sections (H2):

```markdown
## I. Introduction

## II. Canonical Sources

## III. Core Argument

## IV. Cross-Disciplinary Engagement

## V. Middle-Way Corrections

## VI. Conclusion
```

EN sub-sections (H3):

```markdown
### 2.1 Sūtra references

### 2.2 Treatise references

### 3.1 The semantic structure of *X*
```

Roman top + Arabic sub is the canonical pattern. The audit found this is *not* drift — it's the intended bilingual structure, and 22/31 EN papers already use it. The remediation work is converting the 4 P01–P04 EN papers from Arabic top to Roman top.

ZH top-level sections (H2):

```markdown
## 一、引言

## 二、經典依據

## 三、核心論證

## 四、跨學科會通

## 五、中道校正

## 六、結論
```

ZH sub-sections (H3):

```markdown
### 2.1 經文基礎

### 2.2 論疏基礎

### 3.1 *X* 之語意結構
```

ZH sub-sections use Arabic decimal (the same as EN), not 中文數字. This is the existing pattern (audit confirmed 31/31 ZH consistent) and is preserved.

**Body cross-references** to top-level sections:
- EN: `§II`, `§III` (matching Roman heading) for top-level; `§3.1`, `§3.4` for sub-section
- ZH: `§二`, `§三` for top-level; `§3.1`, `§3.4` for sub-section

Rationale: cross-refs to a heading should match that heading's notation. This is what the audit found in 22 EN papers' "mixed" classification — that "mix" is correct, not a defect.

### 2.5 References section (Decision #10)

EN heading: `## References` — single canonical, kill `Bibliography` in P05/P15/P17/P29.

ZH heading: `## 參考文獻` — restore where missing in P24–P28 ZH.

#### 2.5.1 Default sub-heading layout (3-part Roman)

For papers without substantive Chinese commentarial sources:

```markdown
## References

### I. Primary Buddhist Sources

- <Pāli canonical sources>
- <Sanskrit sources>
- <Chinese canonical sources / Taishō>

### II. Modern Scholarship

- <peer-reviewed monographs and articles>

### III. Sūtra & Śāstra Index

| Abbr. | Full title | Catalogue no. | Translator |
|---|---|---|---|
| ... |
```

ZH counterpart:

```markdown
## 參考文獻

### 一、佛教原典 Primary Buddhist Sources

### 二、現代學術著作 Modern Scholarship

### 三、經論索引 Sūtra & Śāstra Index
```

Note: ZH sub-headings use bilingual labels (`一、佛教原典 Primary Buddhist Sources`) for cross-language navigability. The 中文數字 prefix matches the H2 numbering convention.

#### 2.5.2 Expanded sub-heading layout (5-part Roman)

For papers that substantively cite Chinese patriarchal commentaries (the P10–P18 cluster, Tang–Ming masters, treatise commentators):

```markdown
## References

### I. Primary Buddhist Sources

### II. Patriarchal Commentaries

### III. Modern Scholarship

### IV. Cross-Reference to Series Papers

### V. Sūtra & Śāstra Index
```

ZH counterpart:

```markdown
## 參考文獻

### 一、佛教原典 Primary Buddhist Sources

### 二、祖師註疏 Patriarchal Commentaries

### 三、現代學術 Modern Scholarship

### 四、系列論文交叉引用 Cross-Reference to Series Papers

### 五、經論索引 Sūtra & Śāstra Index
```

The 5-part is content-driven, not style-driven. Use it when the paper has ≥3 distinct patriarchal-commentary citations. Otherwise default to 3-part.

#### 2.5.3 Sub-heading naming canonical

These names are locked; do not paraphrase or substitute synonyms:

- `Primary Buddhist Sources` (not `Primary Sources`, `Canonical Sources`)
- `Modern Scholarship` (not `Secondary Scholarship`, `Secondary Literature`)
- `Sūtra & Śāstra Index` (not `Index of Sūtras and Śāstras`, `Canonical Index`)
- `Patriarchal Commentaries` (not `Commentaries`, `Ancient Chinese Commentaries`)
- `Cross-Reference to Series Papers` (not `Cross-References Within the Series`, `Series-Internal Cross-References`)

The audit found 6+ name variants for the same concept. Lock to the listed names.

#### 2.5.4 Cross-Reference Index location

Lives **inside** the References section as a sub-heading (per 5-part layout), **not** as a separate top-level `## Cross-Reference Index` section. P08 EN's separate-section pattern is the outlier; collapse into the References block.

#### 2.5.5 ZH structure mirrors EN structure (sub-section count rule)

The ZH `## 參考文獻` sub-section count and labeling MUST mirror the EN `## References` sub-section count and labeling exactly. **Do not add or remove sub-sections during format remediation**, even if the EN counterpart is currently below the canonical 3-part default.

Rationale: where an EN paper's References section has fewer sub-sections than the canonical default (e.g., P28 EN has only `### I` and `### II`, missing the default `### III. Sūtra & Śāstra Index`), this is a **content-coverage gap, not a format drift**. Format remediation does not author missing index tables or reorganize sub-sections; that is content work, registered separately as audit-known-gap and scheduled for a content-authoring session post-remediation.

**Restoration protocol for missing ZH `## 參考文獻`** (the P24–P28 ZH cohort identified by the 2026-05-04 audit):

Where a ZH file is missing `## 參考文獻` entirely, Phase C imports the EN counterpart's References content verbatim — same sub-section count, same heading structure, same entries. EN-language scholarship entries remain in EN (canonical ZH academic practice); T-numbers + Pinyin + 漢譯經名 are language-neutral; full ZH-form rewrite (per the P15 ZH model — full CJK sūtra titles, ZH translator labels, ZH dating) is content work and out of Phase C's format-only scope.

The first line inside the restored `## 參考文獻` section MUST carry the canonical HTML comment marker:

```html
<!-- CONTENT-PASS REQUIRED: EN-format entries imported from EN counterpart on YYYY-MM-DD; ZH-form rewrite (per P15 ZH model) scheduled as audit-known-gap. -->
```

The marker is visible-in-source, invisible-in-render — it survives file moves and external mirroring (e.g., when papers propagate to the Waken site, where `tasks/NIAN_NEXT.md` does not follow). Register the gap in `tasks/NIAN_NEXT.md` for the separate content-authoring session.

This rule supersedes any ambiguity in §2.5.1's "default" 3-part layout for papers legitimately at 2 sub-sections.

### 2.6 Footer block (Decision #9)

The canonical footer block sits at the very end of the file, separated from body content by a final `---`:

```markdown
---

*CBETA collation: all T-number / X-number citations verified against the CBETA Electronic Tripiṭaka 2024 edition.*

*Repo: github.com/pointing-at-the-moon/four-practice-gates · CC BY-NC-SA 4.0*

*NIAN-P## · 釋慧鏡 (Shi Huijing) · YYYY-MM-DD*
```

Three italic paragraphs in this order:
1. **CBETA collation notice** — *required when the paper substantively cites Taishō / Wàn xùzàng* (defined as: ≥1 T-number, X-number, or A-number citation in body or references). Single-line italic paragraph. May extend to two lines if specific volume/edition disambiguation is required (the P15–P18 pattern). The canonical text is exactly:
   - EN: `*CBETA collation: all T-number / X-number citations verified against the CBETA Electronic Tripiṭaka 2024 edition.*`
   - ZH: `*CBETA 校對：所有 T-number 引文均據 CBETA 電子佛典集成 2024 版實體驗證。*`
   - Append ` / X-number` (EN) or `／X-number` (ZH) before the period if the paper cites X-numbers in addition to T-numbers.
   
   Where existing footers carry a non-canonical CBETA-related statement (e.g., `*CBETA verification: ...*` label variant, plain non-italic English statement, embedded in copyright preamble), Phase E remediation normalizes to the canonical form. Volume/edition disambiguation is preserved when present (the P15 case).
2. **Repo + license line** — *always present*. Single line, italic. Identical across all NIAN papers; can be programmatically inserted.
3. **Attribution (Form B)** — *always present*. Italic, single line.
   - EN: `*NIAN-P## · 釋慧鏡 (Shi Huijing) · YYYY-MM-DD*`
   - ZH: `*NIAN-P## · 釋慧鏡 · YYYY-MM-DD*` (no parenthetical Pinyin in ZH file)

#### Cleanups required by this canonical:

- Form A (`*NIAN-P01 · Shi Huijing · 2026-04-14*` — English-only) in P01/P03 EN: convert to Form B
- Form C (`*NIAN-P05 · 釋慧鏡 · 2026-04-15*` — Chinese-only without Pinyin) in P05 EN: convert to Form B (keep C as-is in ZH file — that's the canonical ZH footer)
- "Form D" long-form copyright statements (P09, P22–P31 ZH; P30/P31 EN) folded into the canonical 3-paragraph block; the long copyright phrasing becomes the second paragraph (repo + license line) in compressed form
- Bare `**Repo:**` or `Series GitHub repository:` lines (P10–P29 various): replaced by the canonical italic repo line
- "Other italic" CBETA-only footers (P12–P18) are missing the attribution line; remediate to add Form B beneath the existing CBETA notice

### 2.7 Coda paper exceptions (P30, P31)

The Part VIII Coda papers carry intentional structural markers above the canonical footer block. These are body elements, not footer variants:

```markdown
[... body content ...]

---

§ 8.1 — NIAN Part VIII Coda Opener · NIAN 30/31

[OR for P31:]

§ 8.2 — NIAN Part VIII Coda Closer · NIAN 31/31 ✅ Volume sealed · Volume One is not the end of the series · The three remaining volumes of *Dāna*, *Śīla*, *Dhyāna* await unfolding.

---

*CBETA collation: ...*

*Repo: github.com/pointing-at-the-moon/four-practice-gates · CC BY-NC-SA 4.0*

*NIAN-P30 · 釋慧鏡 (Shi Huijing) · 2026-05-XX*
```

Notes:
- The `§ 8.X` Coda marker is its own paragraph between two `---` rules
- The orphan `8.1` token at the very end of P30 EN and P30 ZH (after the canonical footer in each) is a build artifact — remove from both files
- The Coda papers are the only papers with this structural pattern; do not propagate to other parts

**Coda papers may also carry a second bold line in the title block**, immediately below the primary bold subtitle, marking the paper's structural position in Part VIII:

```
# Recollection in the Age of AI

**The *Manas*-Turning Operator Still Operative**

**Part VIII Coda · *Buddhānusmṛti* in the Age of AI · Paper 30 of *Mindful of the Buddha*'s thirty-one**

*中文主題：AI 時代之念佛——末那-轉依機制仍然有效*

---
```

This second bold line is permitted ONLY for Coda papers (P30, P31) and ONLY in the title block. It is the structural counterpart of the `§ 8.X` body marker: the §8.X marker names the position before the body-end, the title-block second bold line names the position before the body begins. Both are body-content markers in italic-bold form, not parser-anchor lines. The Waken parser extracts `subtitle_en` from the **first** bold line under H1; the second bold line is treated as supplementary metadata and rendered below the subtitle.

**Part-opener papers may also carry a second bold line in the title block**, immediately below the primary bold subtitle, marking the paper's structural position within its Part. Permitted for the first paper of each Part (typically P01, P05, P06, P09, P12, P20, P25 — and P30 is covered by the Coda exception above). The second bold line names the editorial position; the parser treats the first bold line as the subtitle landmark and the second as supplementary metadata. Same rendering treatment as Coda second-bold-line.

Example (P25 EN):

```
# Samantabhadra's Ten Great Vows

**An Introduction**

**From within the *Avataṃsaka*'s Fortieth Fascicle to Part VI's Confluence as Textual Seam — Part VII's Opening Paper on the Flower Ornament Vision**

*中文主題：普賢十大願·導論*

---
```

The part-opener second bold line is OPTIONAL — papers that don't have a structural-position marker simply omit it. ZH counterparts may carry a translated parallel second bold line; same rendering rules apply.

---

## 3. Punctuation conventions

### 3.1 EN and ZH H1 separator conventions (Phase E v1.2.1 amendment)

**ZH H1 separator list (canonical for Phase E and onward)**: Where ZH H1 is split into title-only + bold-subtitle per §2.1, the recognized separator characters are: ` — ` (space + en-dash + space, U+2014 with ASCII spaces), `——` (CJK em-dash run, two U+2014), `：` (full-width colon, U+FF1A), and ` · ` (space + middle-dot + space, U+00B7) when used as subtitle separator (NOT when used as compound-title separator like `普賢十大願·導論` or `禮敬諸佛・稱讚如來・廣修供養`). The audit script disambiguates by comparing H1 against YAML title — if YAML title uses the same compound form, `·` is treated as compound, not separator.

EN H1 separator list: `: ` (half-width colon + space) and ` — ` (space + em-dash + space). Disambiguation rule for EN: same heuristic — compound vs separator decided by YAML title.

### 3.3 EN files — Type A: transliteration parens (Decision #7)

When a parenthetical inside EN prose contains romanized text, mixed romanized+CJK, or Sanskrit/Pāli IAST, use **half-width parens** `( ... )`.

✓ `*niànfó* (念佛, "buddhānussati")` — mixed content, half-width
✓ `the *Sūtra in Forty-Two Sections* (T0784)` — alphanumeric, half-width
✓ `Lokakṣema (Zhī Lóujiāchèn 支婁迦讖)` — mixed romanized + CJK, half-width
✓ `(*upacāra-samādhi*)` — pure IAST, half-width

When a parenthetical is a **pure-CJK insertion as a CJK insertion** (rare in EN files, but legitimate when citing a Chinese-source phrase block), full-width parens `（ ... ）` may be retained:

✓ `the verse 「念佛即是念心」` — direct CJK quotation, no parens needed
✓ `（公元 67 年）` standing alone as a CJK temporal marker — keep full-width

But:
✗ `（公元 67 年）` embedded in EN prose — convert to `(67 CE)` or `(year 67)` per English convention
✗ `（迦葉摩騰）` parenthetical name — convert to `(Kāśyapa Mātaṅga)` or romanize-and-half-width

**Inner-content normalization applies regardless of paren state.** If parens have already been half-width-converted (e.g., `(公元 67 年)` rather than `（公元 67 年）`), the inner-content normalization rule still applies. Sonnet's Phase B dry-run heuristic ran paren-conversion first and missed the post-conversion form; remediation must apply Chinese-era date stamps and proper-name romanization on a second pass after paren conversion. Specifically:

- `(公元 N 年)` and `（公元 N 年）` → `(N CE)` (e.g., `(公元 67 年)` → `(67 CE)`)
- `(公元 N–N 年)` and `（公元 N–N 年）` → `(N–N CE)` (e.g., `(公元 148–170 年)` → `(148–170 CE)`)
- `(西元 N 年)` and `（西元 N 年）` → `(N CE)`
- `(迦葉摩騰)` and `（迦葉摩騰）` → `(Kāśyapa Mātaṅga)` (Pinyin + IAST per established transliteration in body text)

ZH papers retain `公元 N 年` as the natural ZH form; this rule applies only to EN papers.

The audit found ~640 raw FW-punct hits across 28 EN files; the majority are Type A and resolve to half-width.

### 3.4 EN files — Type B: Chinese-source citation quotes (Decision #8)

When an EN sentence cites a specific Chinese-source phrase or term verbatim, **keep the `「...」` quotation marks intact**:

✓ `the Chinese term 「他力」 predates Shinran by centuries`
✓ `the fourteen characters 「念佛即是念心，求心即是求佛」 — *recollecting the Buddha is recollecting the mind*`
✓ `the precept-body's 「離色心、離有無、離因果」 negation`

Rationale: these are scholarly citations, not stylistic quirks. `「他力」` is the cited term being discussed; converting to `"他力"` (ASCII double quote) loses the visual cue that this is a Chinese-source citation, and converting to `"tālì"` loses the orthographic ground of the discussion. Keep `「...」` when (a) the content inside is Chinese-source verbatim and (b) the surrounding sentence frames it as a citation.

The audit found this pattern dominant in P20–P21 (the patriarch-cluster papers). Do not "fix" these.

#### Boundary heuristic for Type A vs Type B

When in doubt:
- If the parens contain a translation, gloss, transliteration, or alphanumeric content → Type A → half-width
- If the marks are `「」` and the content is verbatim Chinese-source → Type B → keep
- If the parens are `（）` and the content is Chinese-source quotation → likely Type A drift; convert to `「」` (Type B convention)

### 3.5 EN files — full-width comma `，` and period `。`

These have no place in EN prose. Convert to `,` and `.`. The audit found these only inside Type A parens (`（迦葉摩騰）` → `（迦葉摩騰， T0784）`); they go away when Type A is fixed.

### 3.6 ZH files

ZH files use full-width punctuation throughout: `，。、；：「」『』（）——……`

No half-width punctuation in ZH prose **except**:
- ASCII for T-numbers, citation references (`T0784`, `Vism VII.1`)
- ASCII for code paths, URLs, English passages quoted verbatim
- ASCII inside YAML frontmatter and footer attribution lines (handled by the YAML/footer convention sections)

The audit confirmed ZH files have 0 hits on full-width-in-EN issues; ZH punctuation is consistent. No ZH punctuation remediation required.

---

## 4. Parser landmarks (Waken site contract)

This section names the structural landmarks that the future Waken `lib/papers.ts` parser variant for four-practice-gates papers will rely on. Format remediation must preserve these landmarks exactly.

### 4.1 Filename pattern

```
{partN_slug}/P{##}_{english-slug}.md     (EN)
{partN_slug}/P{##}_{中文-slug}.md         (ZH)
```

Regex: `^P(\d{2})_(.+)\.md$`

The Waken parser identifies the language from the slug (ASCII = en, contains CJK = zh). Filename format is locked and must not change during remediation.

### 4.2 YAML frontmatter

Parser uses standard YAML library; `pyyaml` or equivalent. All fields in §1.1 must be present and parseable. Anomaly #9 (bare-asterisk alias) must be remediated for all 62 files even if not currently breaking other tooling.

### 4.3 H1 + subtitle landmarks

```
# Title
[blank line]
**Bold subtitle**
[blank line]
*中文主題：...*       (EN files only)
[blank line]
---
```

The parser extracts:
- `title_en` from H1 of EN file
- `title_zh` from H1 of ZH file
- `subtitle_en` from the bold line of EN file
- `subtitle_zh` from the bold line of ZH file

The blank lines between elements are the parser's structural cues; do not collapse them.

### 4.4 Author block landmarks

```
**Author:** 釋慧鏡 (Shi Huijing)
**Date:** April 2026
**License:** CC BY-NC-SA 4.0
**Version:** 1.0
**Series:** NIAN — Mindful of the Buddha

---
```

The parser may double-check YAML metadata against this human-readable block; mismatches will produce a build warning. They should not exist after remediation.

### 4.5 Abstract landmark

```
## Abstract                  (EN only)
## 摘要                       (ZH only)
```

The parser extracts the first paragraph after this heading as the abstract body. Word-count contracts are preserved (EN: 30–1200 words; ZH: 50–1500 chars per Waken paper-schema).

### 4.6 References section landmark

```
## References                (EN only)
## 參考文獻                   (ZH only)
```

The parser extracts the references-section content as a separate field. Sub-headings (`### I. Primary Buddhist Sources` etc.) are preserved in markdown form for rendering.

### 4.7 Footer landmarks

The canonical footer block (§2.6) is parser-anchored on:
- The final `---` rule before the footer
- The italic attribution line `*NIAN-P## · 釋慧鏡 (Shi Huijing) · YYYY-MM-DD*`

The parser extracts:
- `paper_id` from the attribution line (cross-checked against YAML)
- `date` from the attribution line (cross-checked against YAML; YAML wins on mismatch)

---

## 5. Pre-commit validation checklist

Every paper edited under remediation (or written de novo for SĪLA / DĀNA / DHYĀNA) must pass:

- [ ] YAML parses with standard pyyaml (`yaml.safe_load`) without exception
- [ ] All 7 YAML fields (`title`, `author`, `series`, `paper_id`, `date`, `version`, `license`) are present and quoted
- [ ] `paper_id` matches `^NIAN-P(0[1-9]|[12]\d|3[01])$`
- [ ] `date` matches `^\d{4}-\d{2}-\d{2}$`
- [ ] `version` matches `^\d+\.\d+$`
- [ ] `series` matches the canonical EN or ZH string (per §1.6)
- [ ] H1 line present and matches title in YAML
- [ ] Bold-subtitle line present immediately after H1 (with blank-line separator)
- [ ] EN file has `*中文主題：…*` line with full-width colon (or omitted intentionally for Coda papers)
- [ ] First `---` rule closes the title block
- [ ] Author-metadata block present with all 5 lines (`Author`, `Date`, `License`, `Version`, `Series`)
- [ ] Second `---` rule closes the author block
- [ ] `## Abstract` (EN) or `## 摘要` (ZH) heading present
- [ ] `**Keywords:**` (EN) or `**關鍵詞：**` (ZH) present after the abstract paragraph
- [ ] H2 top-level sections use Roman numerals (EN) / 中文數字 (ZH)
- [ ] H3 sub-sections use Arabic decimal in both languages
- [ ] `## References` / `## 參考文獻` heading present
- [ ] Sub-headings under References use the canonical names (§2.5.3)
- [ ] Body has no full-width parens around romanized content (Type A)
- [ ] Body preserves `「...」` quotes around Chinese-source citations (Type B)
- [ ] Footer block has 3 italic paragraphs in the order: CBETA (optional) → repo line → attribution
- [ ] Attribution line matches Form B exactly
- [ ] No orphan tokens after the footer
- [ ] If `## 參考文獻` was restored from EN counterpart in this remediation pass, the first line inside the section is the canonical HTML comment marker (per §2.5.5)

For Coda papers (P30, P31): additionally verify the `§ 8.X` marker block is present and properly delimited by `---` rules, and the orphan `8.1` token at end of P30 EN and P30 ZH is removed from both.

---

## 6. What this document does NOT specify

These remain author/Code judgment, not canonicalized here:

- Body paragraph length, sentence rhythm, prose register
- Footnote style (existing `[^N]` pattern continues)
- Block-quote conventions (existing `>` markdown continues)
- Cross-paper reference syntax (`P15`, `S4-P05`, etc. — existing inline parenthetical)
- Tables (existing markdown tables continue)
- Image / figure handling (none in NIAN; reserved for SĪLA/DĀNA if needed)
- Coda paper §8.X enumeration logic (kept as-is per Decision #9)

These conventions live in `PAPER_STYLE.md` (the four-practice-gates project's style guide) and are inherited, not redefined here.

---

## 7. Authority and supersession

This canonical is **v1.1**, dated 2026-05-04. v1.0 was published earlier the same day at commit `0755953`; v1.1 followed Phase B dry-run review which surfaced four clarification items requiring canonical patching before Phase C dispatch.

**Revision history**

- **v1.0** (2026-05-04, commit `0755953`): initial release; 10 decisions locked.
- **v1.1** (2026-05-04): clarifications surfaced by Phase B dry-run review of P05 EN, P15 EN/ZH, P28 EN/ZH:
  - §1.7 (new): ZH-only optional YAML fields (`part`, `sequence`, `language: zh-Hant`) recognized as legitimate optional metadata
  - §2.5.5 (new): ZH `## 參考文獻` sub-section count and structure mirrors EN `## References` exactly; do not add sub-sections during format remediation, even if EN is below the canonical 3-part default. Restoration protocol with HTML comment marker.
  - §3.1 (clarified): Chinese-era date stamps (`公元 N 年`, `公元 N–N 年`, `西元 N 年`) normalize to `(N CE)` regardless of paren state — applies to post-paren-conversion form as well
  - §5 (added): HTML comment marker required when ZH `## 參考文獻` is restored from EN counterpart

- **v1.2** (2026-05-04, post-Phase-D): Coda-paper title-block second bold line codified as permissible structural exception for P30/P31 only (§2.7 amendment + §0 quick-ref row 11). Phase E discovery and fix sub-phase prevents reintroduction of drift in this slot.

- **v1.2.1** (2026-05-04, post-Phase-E-discovery): three clarifying amendments surfaced by Phase E sub-phase 1 discovery:
  - §2.7 generalized: title-block second bold line permitted for Part-opener papers (P01, P05, P06, P09, P12, P20, P25), parallel to existing Coda exception
  - §3.1 ZH separator list explicit: `：` (U+FF1A) added as recognized H1 subtitle-separator; compound-vs-separator disambiguation via YAML title comparison
  - §2.6 CBETA notice tightened from "optional" to "required when ≥1 T/X/A citation"; canonical templates fixed; existing non-canonical variants normalize during remediation

Future revisions:
- `v1.x`: clarifications, typo fixes, additional examples — same conventions
- `v2.0`: substantive convention change (e.g., switching from Roman to Arabic top-level, or restructuring References into 4-part) — requires explicit author approval and produces a remediation pass for all volumes already shipped

The remediation pass dispatched against this v1.0 spec produces the **NIAN format-stable corpus** that propagates to:
- `PAPER_STYLE.md v2.2` of `four-practice-gates` repo (this canonical's content folds in as an authoritative §X)
- The first papers of SĪLA, DĀNA, DHYĀNA (which are written *to* this canonical from the start)
- The Waken parser variant for `/practice/papers/nian/` (which depends on the landmarks of §4)

---

*釋慧鏡 · 指月 · 四行門 · NIAN canonical format · v1.2.1 · 2026-05-04*
