# Content Redundancy Report - Vanna Documentation

**Scope:** `learn/` (Core Concepts) vs `guides/` (User Guide)  
**Purpose:** Identify where content is duplicated or overlapping, and how to fix it.

---

## How to Read This Report

Each issue shows:
- **Files involved** with the specific sections/lines that overlap
- **What's duplicated** - the actual repeated content
- **Fix** - concrete action to take

Severity: `HIGH` = meaningful duplication that confuses the reader | `MEDIUM` = some overlap but distinct enough | `LOW` = minor inconsistency

---

## HIGH Severity

---

### 1. Health Factor Formula - Defined Twice

**Files:**
- [learn/health-factor.mdx](learn/health-factor.mdx) - Lines 12–15 (LaTeX formula + table)
- [guides/margin/health-factor.mdx](guides/margin/health-factor.mdx) - Lines 18–22 (code block formula)
- [guides/margin/overview.mdx](guides/margin/overview.mdx) - Line 57 (inline in table)

**What's duplicated:**  
The formula `Health Factor = Total Collateral Value / Total Debt Value` and the status table (Safe / Caution / Liquidatable with 1.1× and 1.5× thresholds) appear in full in both the concepts page and the user guide page.

**Fix:**  
The `guides/margin/health-factor.mdx` should show the formula once, then link to `learn/health-factor.mdx` for the full mathematical treatment. Remove the status table from the guides page - replace it with a one-line description and a "See Core Concepts for full breakdown →" link. The `guides/margin/overview.mdx` table row showing `Health Factor = collateral / debt` is fine since it's in context of a metrics table.

---

### 2. "What Moves Your Health Factor" - Two Full Lists

**Files:**
- [learn/health-factor.mdx](learn/health-factor.mdx) - Lines 122–134 (table with 9 events)
- [guides/margin/health-factor.mdx](guides/margin/health-factor.mdx) - Lines 45–59 (bullet list split into "goes down / goes up")

**What's duplicated:**  
Both pages enumerate the same set of events (collateral price drop, borrow more, repay debt, collateral price rise, interest accrual) and their effect on health factor. The content is structurally identical, just formatted differently.

**Fix:**  
Keep the full table in `learn/health-factor.mdx` (the canonical technical reference). In `guides/margin/health-factor.mdx`, replace the full list with a condensed 2-sentence summary and link: "Your Health Factor changes whenever collateral prices move, you borrow or repay, or interest accrues. [See the full breakdown →](/learn/health-factor#what-moves-your-health-factor)"

---

### 3. Liquidation Trigger Explanation - Repeated in Three Places

**Files:**
- [learn/liquidation.mdx](learn/liquidation.mdx) - Lines 9–18 (four trigger routes)
- [guides/margin/liquidation.mdx](guides/margin/liquidation.mdx) - Lines 9–13 (same triggers, prose form)
- [guides/margin/overview.mdx](guides/margin/overview.mdx) - Lines 40–41 (inline explanation)

**What's duplicated:**  
All three pages explain that liquidation is triggered when Health Factor reaches 1.1×, that it's permissionless, and that collateral price drop / interest accrual are the main causes.

**Fix:**  
- `learn/liquidation.mdx` - keep as the full technical reference (this is the canonical page).
- `guides/margin/liquidation.mdx` - reduce the trigger explanation to 2 sentences, link to learn page: "Liquidation triggers when your Health Factor reaches 1.1×. [What can cause that →](/learn/liquidation#when-liquidation-becomes-possible)"
- `guides/margin/overview.mdx` - the inline mention is fine in context, no change needed.

---

### 4. Liquidation Step-by-Step - Duplicated Across Concepts + Guide

**Files:**
- [learn/liquidation.mdx](learn/liquidation.mdx) - Lines 52–68 (`<Steps>` component: Detection → Initiation → Debt repayment → Collateral sweep → Account closure)
- [guides/margin/liquidation.mdx](guides/margin/liquidation.mdx) - Lines 19–38 (`<Steps>` component: Health Factor touches 1.1× → Liquidator submits → Collateral transferred → Debt repaid → Fee deducted → Remainder returned)

**What's duplicated:**  
Both pages have a step-by-step breakdown of the liquidation process. The concepts page is protocol-oriented (contract calls), the guide is user-oriented (what happens to you). Steps 2–4 substantially overlap in what they describe.

**Fix:**  
These serve genuinely different audiences, so both can exist - but they need explicit differentiation:
- Add a callout box at the top of `guides/margin/liquidation.mdx`: "This page explains what liquidation means for you as a user. [For the technical contract-level flow →](/learn/liquidation)"
- Add a callout in `learn/liquidation.mdx`: "For the user-facing perspective (how to avoid it, what to do if imminent), see [Guides: Liquidation →](/guides/margin/liquidation)"
- This cross-linking is currently absent on both pages.

---

### 5. LP Yield Mechanism - Explained in Both Concepts + Guide

**Files:**
- [learn/lending-pools.mdx](learn/lending-pools.mdx) - Full page explaining pools, two sides (LP/borrower), isolation, risk
- [guides/earn/overview.mdx](guides/earn/overview.mdx) - Lines 12–19 explain the same mechanism: supply → vTokens → interest accrues → exchange rate rises → withdraw = original + yield

**What's duplicated:**  
Both pages explain the core LP mechanic: deposit assets → receive vTokens → borrower interest raises the exchange rate → withdraw more than deposited. The `guides/earn/overview.mdx` already has a cross-link to concepts ("To understand how vTokens, exchange rates, and the interest rate model work in depth, see Lending Pools and vTokens in Core Concepts"), but the explanation before that link duplicates what's in concepts.

**Fix:**  
In `guides/earn/overview.mdx`, trim the "How lending pools work in Vanna" section to 2–3 sentences of plain-English summary, then immediately point to the concepts cross-link. The current paragraph (Lines 12–19) is too long and overlaps with `learn/lending-pools.mdx` and `learn/vtokens.mdx`.

---

## MEDIUM Severity

---

### 6. The Flywheel Concept - Explained Twice in Guides

**Files:**
- [guides/how-vanna-works.mdx](guides/how-vanna-works.mdx) - Lines 16–20 (The Vanna Flywheel section with diagram)
- [guides/earn/overview.mdx](guides/earn/overview.mdx) - Lines 27–31 ("Higher utilization from undercollateralized leverage" + "Dynamic interest rates" sections)

**What's duplicated:**  
Both pages explain the same economic loop: undercollateralized leverage → higher utilization → higher yields for LPs → more capital → lower borrowing costs → more traders. This isn't concepts vs. guides - it's the same idea repeated twice within the guides section itself.

**Fix:**  
In `guides/earn/overview.mdx`, replace the utilization explanation under "Key factors driving LP earnings" with a single sentence that links to the flywheel: "Vanna's undercollateralized leverage model structurally keeps utilization - and therefore your yields - higher than standard money markets. [See how the Vanna Flywheel works →](/guides/how-vanna-works#the-vanna-flywheel)" Remove the 3-paragraph "Key factors driving LP earnings" expansion and keep only the liquidation fee point, which is not covered by the flywheel page.

---

### 7. Permissionless Liquidation Explanation - Repeated

**Files:**
- [learn/liquidation.mdx](learn/liquidation.mdx) - Lines 83–85 ("Why Permissionless" section, detailed)
- [guides/margin/liquidation.mdx](guides/margin/liquidation.mdx) - Lines 12–13 (one sentence: "It is permissionless... Liquidators are economically incentivized...")

**What's duplicated:**  
Both pages explain that anyone can liquidate and that liquidators are economically incentivized. The guides page version is brief, but the two-sentence overlap is still redundant given the cross-link fix described in issue #4.

**Fix:**  
After fixing issue #4 (adding cross-links between the pages), the brief mention in the guides page becomes a deliberate, minimal summary pointing elsewhere. No additional change needed beyond what #4 prescribes.

---

### 8. 1.1× Threshold - Over-Explained Across Pages

**Files:**
- [learn/health-factor.mdx](learn/health-factor.mdx) - Threshold defined, visualized, explained with "Why 1.1× and Not 1.0×" section
- [guides/margin/health-factor.mdx](guides/margin/health-factor.mdx) - Threshold explained multiple times in prose
- [guides/margin/overview.mdx](guides/margin/overview.mdx) - "must stay above 1.1× to avoid liquidation" in table and prose
- [guides/margin/liquidation.mdx](guides/margin/liquidation.mdx) - threshold restated multiple times

**What's duplicated:**  
The 1.1× threshold is a key concept but is re-explained from scratch on every page it appears on, rather than being stated once and linked.

**Fix:**  
The full explanation of why 1.1× belongs only in `learn/health-factor.mdx` (the "Why 1.1× and Not 1.0×" section). All other pages should state the threshold value and link to the concepts page, not explain it again.

---

### 9. Borrow Stays Inside Margin Account - Repeated

**Files:**
- [guides/margin/overview.mdx](guides/margin/overview.mdx) - Line 42: "Borrowed assets stay inside your Margin Account. You can deploy them to Farm or Trade from within the account, but they cannot be sent directly to your regular wallet."
- [guides/margin/borrow.mdx](guides/margin/borrow.mdx) - likely restates this constraint in the borrow flow (verify in file)

**What's duplicated:**  
The constraint that borrowed capital stays in the Margin Account and cannot be sent to your wallet is an important rule that gets re-stated in multiple guide pages.

**Fix:**  
State it once in `guides/margin/overview.mdx` (already there). In `guides/margin/borrow.mdx`, replace any re-statement with a short note: "Borrowed assets remain in your Margin Account - [learn why →](/guides/margin/overview)"

---

## LOW Severity

---

### 10. Liquidation Fee Wording Inconsistency

**Files:**
- [guides/earn/overview.mdx](guides/earn/overview.mdx) - Line 30: "**Liquidation penalties shared with LPs**" / "liquidation penalty is distributed to the lending pool"
- [guides/margin/liquidation.mdx](guides/margin/liquidation.mdx) - Lines 43–46: "liquidation fee is distributed to liquidity providers"

**What's inconsistent:**  
The user guide for LPs calls it a "liquidation penalty" while the liquidation page (written from the borrower's perspective) calls it a "liquidation fee." These refer to the same thing.

**Fix:**  
Standardize to **"liquidation fee"** across all pages. Update `guides/earn/overview.mdx` Line 30 to use "fee" not "penalty." "Penalty" implies punishment to the borrower (not neutral enough for a user-facing LP guide); "fee" is more accurate.

---

### 11. Missing Cross-Links - Health Factor ↔ Liquidation

**Files:**
- [guides/margin/health-factor.mdx](guides/margin/health-factor.mdx) - "Related" section links to `/guides/margin/liquidation` ✓
- [guides/margin/liquidation.mdx](guides/margin/liquidation.mdx) - "Related" section links to `/guides/margin/health-factor` ✓
- [learn/health-factor.mdx](learn/health-factor.mdx) - "Related" section links to `/learn/liquidation` ✓
- [learn/liquidation.mdx](learn/liquidation.mdx) - "Related" section links to `/learn/health-factor` ✓

**But missing:**  
Neither concepts page links to its corresponding guides page, and neither guides page links to its corresponding concepts page. A reader coming from `guides/margin/health-factor.mdx` has no path to `learn/health-factor.mdx` and vice versa.

**Fix:**  
Add cross-section links to the "Related" section of each page:
- In `learn/health-factor.mdx` Related: add "User Guide: Health Factor → /guides/margin/health-factor"
- In `learn/liquidation.mdx` Related: add "User Guide: Liquidation → /guides/margin/liquidation"
- In `guides/margin/health-factor.mdx` Related: add "Core Concepts: Health Factor → /learn/health-factor"
- In `guides/margin/liquidation.mdx` Related: add "Core Concepts: Liquidation → /learn/liquidation"

---

## Summary Table

| # | Issue | Files | Severity | Fix Type |
|---|---|---|---|---|
| 1 | Health Factor formula defined twice | `learn/health-factor`, `guides/margin/health-factor` | HIGH | Remove from guide, add link |
| 2 | "What moves Health Factor" listed twice | Same two files | HIGH | Condense guide version, link to concepts |
| 3 | Liquidation trigger explained in 3 places | `learn/liquidation`, `guides/margin/liquidation`, `guides/margin/overview` | HIGH | Condense guide versions, cross-link |
| 4 | Liquidation step-by-step in both sections | `learn/liquidation`, `guides/margin/liquidation` | HIGH | Both are valid - add cross-section callouts |
| 5 | LP yield mechanism explained twice | `learn/lending-pools`, `guides/earn/overview` | HIGH | Trim guide version, expand cross-link |
| 6 | Flywheel explained twice within guides | `guides/how-vanna-works`, `guides/earn/overview` | MEDIUM | Replace earn overview section with link |
| 7 | Permissionless liquidation repeated | `learn/liquidation`, `guides/margin/liquidation` | MEDIUM | Resolved by fix #4 |
| 8 | 1.1× threshold over-explained everywhere | Multiple files | MEDIUM | Full explanation only in concepts, link elsewhere |
| 9 | Borrow stays in account - repeated | `guides/margin/overview`, `guides/margin/borrow` | MEDIUM | State once, link on other pages |
| 10 | "Penalty" vs "Fee" inconsistency | `guides/earn/overview`, `guides/margin/liquidation` | LOW | Standardize to "liquidation fee" |
| 11 | Missing concepts ↔ guides cross-links | All health factor + liquidation pages | LOW | Add cross-section links to Related sections |

---

## General Principle for Future Content

The pattern causing most redundancy is: **a concept being re-explained from scratch instead of referenced.**

The intended structure is:
- **`learn/`** = canonical definition, full depth, technical detail. Every page here should read as the single source of truth.
- **`guides/`** = what the user needs to act. Brief explanation of the concept, then instructions. Should reference `learn/` for depth, not re-explain it.

Guides pages that currently re-explain concepts should follow this template:
```
[1 paragraph: plain-English what this is]
[Link to Core Concepts for the full breakdown]
[Step-by-step instructions / what to monitor / what to do]
```
