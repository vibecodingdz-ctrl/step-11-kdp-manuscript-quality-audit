---
name: "step-11-kdp-manuscript-quality-audit"
description: "Universal agent skill to audit a KDP manuscript against the Product Brief, Outline, and Quality Contract (factual APA support, originality <6-8%, brief promises, 6-part chapter structure, 30,000-word target) and issue a strict PASS / FAIL / FAIL-INCOMPLETE verdict before design and publishing."
---
---
name: Step_11_kdp-manuscript-quality-audit
description: Universal agent skill to audit a KDP manuscript against the Product Brief, Outline, and Quality Contract (factual APA support, originality <6-8%, brief promises, 6-part chapter structure, 30,000-word target) and issue a strict PASS / FAIL / FAIL-INCOMPLETE verdict before design and publishing.
---

# KDP Manuscript Quality Audit (`Step_11_kdp-manuscript-quality-audit`)

**Role:** Quality & Fact-Checking Agent  
**Purpose:** Audit a completed (or checkpointed) KDP manuscript item-by-item against the Product Brief (`Step_08`), Book Outline (`Step_09`), and Course Quality Contract, outputting a verifiable `PASS`, `FAIL`, or `FAIL-INCOMPLETE` report.

---

## 1. Environment Adaptation & Required Inputs
- **Required Upstream Inputs:**
  1. `08_Book_Development/MANUSCRIPTS/<shortname>_vNN.md` *(Exact version to audit)*
  2. `07_Product_Strategy/brief_<shortname>_<yyyymmdd>.md` *(Dated lowercase filename—never `BRIEF_..._vNN.md`)*
  3. `08_Book_Development/OUTLINES/outline_<shortname>_vNN.md` *(Approved structure & word budgets)*
  - *If the manuscript or brief cannot be read:* Stop and output a `BLOCKED` block (`Error: / Tried: / Next:`).
- **Output Delivery:**
  - **Workspace Agents (Manus, Hermes):** Save the audit report to `08_Book_Development/QUALITY_CHECKS/qc_<shortname>_<yyyymmdd>.md`. **Never overwrite an existing QC file or edit the manuscript during an audit.**
  - **Web / Chat Agents (Claude Projects, Genspark):** Output the complete Markdown QC report directly in the chat using the exact template in Section 4.

---

## 2. Step-by-Step Quality Audit Procedure

### Step 1: Pin the Exact Manuscript Version & Scope
1. **Execution Order (Avoid the Timing Trap):** Ensure all manuscript edits and final word-count updates are 100% finished **before** recording version details. If the manuscript is edited after pinning, the audit is invalid and must be re-run.
2. Record the exact manuscript path, version (`vNN`), file size (in bytes), last-modified date, and SHA-256 hash (if available in the environment or provided by Samir; otherwise write `hash: not available`).
3. **Partial-Manuscript Scope Rule:** If auditing a partial draft (e.g., a 10k or 20k checkpoint), explicitly state `"Scope: Partial manuscript (Chapters X–Y only)"` in the header and mark all book-level items (30,000-word total, description presence, full benefit map, complete APA references) as **`FAIL-INCOMPLETE`**. Never let a partial draft inherit an overall `PASS`.

### Step 2: Run the 7 Mandatory Audit Checks
Every single check must name the specific tool/method used and its measured result:

1. **Factual Claims & APA Support:**
   - Extract every factual, scientific, statistical, or study-based claim in the manuscript.
   - Verify that each claim has a matching, real citation in `## References (APA)`.
   - List every unbacked claim or remaining `[WRITER TODO]` tag individually under `## Unsupported claims`. **Never invent a citation to fix an unsupported claim.** Any unsupported claim = **`FAIL`**.
2. **Originality & Plagiarism Ceiling (`< 6–8%`):**
   - Check for copied strings against competitor descriptions/look-insides and course notes, and run a free web plagiarism checker (or flag if unavailable).
   - Record the **tool name**, **exact match %**, and **capture date**.
   - Match `< 6–8%` = **`PASS`**; Match `≥ 8%` = **`FAIL`**; No free plagiarism checker accessible = **`FAIL-INCOMPLETE`**.
3. **Product Brief Promise Mapping:**
   - Cross-check every promised benefit and deliverable in `brief_<shortname>_<yyyymmdd>.md` against actual sections in the manuscript. Any unfulfilled promise = **`FAIL`**.
4. **Chapter Structure (6 Mandatory Blocks):**
   - Confirm every body chapter contains: (1) Real attributed opening quote, (2) Hook, (3) "What this chapter covers", (4) 80/10/10 main content, (5) 3–6 bullet takeaways, and (6) CTA transition to the next chapter `[COURSE]`.
5. **Readability, Formatting & Writing Bans:**
   - Confirm conversational simple English, subheadings breaking up long text blocks, **zero hyperlinks / `"Click here"` text**, and **zero basic/online-trivial filler** `[COURSE]`. Uses free grammar/readability check (e.g., LanguageTool free web or built-in inspection).
6. **Book Description Presence (`300–400 words`):**
   - Confirm the 300–400 word book description is present and follows the course hook-pain-benefit-CTA structure `[COURSE]`.
7. **Total Word Count (`30,000 words` Target):**
   - Count total words (body + 300–400 word description) using the editor or a free word counter. Record the exact count and tool used `[COURSE]`.

### Step 3: Issue the Overall Verdict
- **`PASS`:** Every single checklist item achieved `PASS`. (Unlocks `Step_12` & `Step_13`).
- **`FAIL`:** One or more checklist items failed (e.g., unsupported claims, missing chapter blocks, word count short).
- **`FAIL-INCOMPLETE`:** No item failed outright, OR any required check/tool was unavailable or the manuscript is partial. *(An overall verdict can **never** be `PASS` if any single row is `FAIL` or `FAIL-INCOMPLETE`).*

---

## 3. Strict Compliance & Honesty Rules
- **Zero Grade Inflation:** Never mark an item `PASS` without running the check and naming the tool/output. Never guess a plagiarism percentage, word count, or citation.
- **Free Tools Only:** Use free editor/browser checks and free web tools (e.g., LanguageTool free). Never use paid tools (Grammarly Premium, Turnitin, Originality.ai) without Samir's written approval—if a check requires a paid tool and no free option works, mark that row `FAIL-INCOMPLETE`.

---

## 4. Required Output Format

Save to: `08_Book_Development/QUALITY_CHECKS/qc_<shortname>_<yyyymmdd>.md`

```markdown
# Quality Check — <Book Title>
<!-- skill: Step_11_kdp-manuscript-quality-audit -->
Shortname: <shortname>
Manuscript audited: 08_Book_Development/MANUSCRIPTS/<shortname>_v<NN>.md (size: <bytes>, last modified: <YYYY-MM-DD>, hash: <sha256 or "not available">)
Scope: <Full manuscript | Partial manuscript (Chapters X–Y only)>
Brief: 07_Product_Strategy/brief_<shortname>_<yyyymmdd>.md
Outline: 08_Book_Development/OUTLINES/outline_<shortname>_v<NN>.md
Audit date: <YYYY-MM-DD>
Auditor: <Agent Name> (Quality & Fact-Checking Role)
OVERALL VERDICT: <PASS | FAIL | FAIL-INCOMPLETE>

## 1. Item Verdicts
| # | Audit Item | Checker / Tool Used | Measured Result | Verdict (`PASS` / `FAIL` / `FAIL-INCOMPLETE`) | Reason / Evidence |
|---|---|---|---|---|---|
| 1 | Factual claims sourced in APA | Manuscript vs. References check | <n> supported, <m> unsupported | <Verdict> | <Reason> |
| 2 | Originality / Plagiarism (<6–8%) | <Tool name + date> | <X.X% or tool unavailable> | <Verdict> | <Reason> |
| 3 | Brief promises fulfilled | Brief vs. Manuscript map | <n> of <total> mapped | <Verdict> | <Reason> |
| 4 | 6-part chapter structure & 80/10/10 mix | Chapter-by-chapter inspection | <All present / Missing in Ch X> | <Verdict> | <Reason> |
| 5 | Readability & writing bans (no links/fluff) | <Tool / link & filler scan> | <0 links, subheadings verified> | <Verdict> | <Reason> |
| 6 | Book description (300–400 words) | Word counter + structure check | <N> words | <Verdict> | <Reason> |
| 7 | Total word count (~30,000 words) | <Word counter used> | <N> total words (<M> body) | <Verdict> | <Reason> |

## 2. Unsupported Claims & Open `[WRITER TODO]` Items
1. **Chapter <X> (<Heading/Paragraph>):** "<Quoted claim>" — Status: `UNSUPPORTED` (Missing APA reference / has `[WRITER TODO]`).
*(If 0 unsupported claims exist, state: "0 unsupported claims found; all factual claims verified against ## References (APA).")*

## 3. Originality Report
- **Tool Used:** <Tool name or "Unavailable">
- **Capture Date:** <YYYY-MM-DD>
- **Measured Match %:** <X.X% or "no data — FAIL-INCOMPLETE"> (Target: `< 6–8%` `[COURSE]`)

## 4. Brief Promises → Manuscript Map
| Promised Benefit / Deliverable (from Brief) | Manuscript Location (Chapter & Subheading) | Status (`Fulfilled` / `Missing`) |
|---|---|---|
| <Promise 1> | Chapter <X> — <Subheading> | <Fulfilled / Missing> |
| <Promise 2> | Chapter <Y> — <Subheading> | <Fulfilled / Missing> |

## 5. Open FAIL / FAIL-INCOMPLETE Items (Required Fixes Before Re-Audit)
- [ ] <Specific action item 1 required to reach PASS>
- [ ] <Specific action item 2 required to reach PASS>
# 

