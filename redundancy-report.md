# Vanna Protocol Docs — Redundancy & Consolidation Report

> Reviewed by: Senior Technical Editor & Documentation Architect  
> Date: 2026-06-04  
> Scope: All `.mdx` files in `Vanna_Docs_Final`

---

## Overview

The Vanna docs are well-structured but carry significant content duplication across the User Guide and Core Concepts sections. The most common pattern is **full concept re-explanation** — instead of explaining something once and linking to it, several pages re-derive the same concept from scratch. This inflates maintenance cost (every concept has multiple sources of truth), and creates reader confusion when the two versions differ slightly in wording or emphasis.

Total redundancies identified: **11**  
Priority breakdown: **4 High · 4 Medium · 3 Low**

---

## Redundancy Index

| # | Title | Files Involved | Priority |
|---|---|---|---|
| R1 | Liquidation explained twice end-to-end | `learn/liquidation.mdx` · `guides/margin/liquidation.mdx` | High |
| R2 | Health Factor formula duplicated | `learn/health-factor.mdx` · `guides/margin/health-factor.mdx` | High |
| R3 | Lending pool mechanics spread across three files | `learn/lending-pools.mdx` · `learn/lending-pool-mechanics.mdx` · `guides/earn/overview.mdx` | High |
| R4 | "Why LP yields are higher" written three times | `guides/how-vanna-works.mdx` · `guides/earn/overview.mdx` · `guides/for-liquidity-providers.mdx` | High |
| R5 | Margin Account capabilities listed in two places | `guides/for-traders.mdx` · `guides/margin/overview.mdx` | Medium |
| R6 | vToken mechanics re-explained in earn guide | `learn/vtokens.mdx` · `guides/earn/overview.mdx` | Medium |
| R7 | Permissionless liquidation explained twice | `learn/liquidation.mdx` · `guides/margin/liquidation.mdx` | Medium |
| R8 | "Borrowed assets stay in Margin Account" repeated | `guides/margin/borrow.mdx` · `guides/margin/overview.mdx` · `guides/for-traders.mdx` | Medium |
| R9 | LP supply flow described in overview and supply guide | `guides/earn/overview.mdx` · `guides/earn/supply.mdx` | Low |
| R10 | "1.1× threshold" explanation restated in every margin page | 6+ files across `guides/margin/` | Low |
| R11 | How to avoid liquidation duplicated | `guides/margin/liquidation.mdx` · `guides/margin/health-factor.mdx` | Low |

---

## Detailed Findings

---

### R1 — Liquidation Explained Twice End-to-End (High)

**Files:** `learn/liquidation.mdx` · `guides/margin/liquidation.mdx`

**What overlaps:**
Both pages define what liquidation is, when it triggers (HF < 1.1×), the permissionless nature, the step-by-step process, what happens to the owner's remaining collateral, and how the liquidation fee works. The core explanation is repeated in full.

**Why it is a problem:**  
Two sources of truth for the same process. When liquidation mechanics change (fee structure, threshold, settlement option), both files must be updated. The learn page goes deeper on economic incentives and the mermaid flowchart; the guides page adds the user-facing "how to avoid" section. Currently neither page links to the other.

**Recommended approach — Differentiate, not duplicate:**

| Page | Should contain | Should remove |
|---|---|---|
| `learn/liquidation.mdx` | Protocol mechanics, mermaid flow, liquidator economics, settlement concept, bad debt handling, why permissionless | Step-by-step user-facing process (that lives in guides) |
| `guides/margin/liquidation.mdx` | User-facing steps, "how to avoid," "what to do if imminent," fee impact on users | Re-derivation of the protocol mechanics — add a single link: *"For the full protocol-level liquidation flow, see [Liquidation in Core Concepts](/learn/liquidation)."* |

**Estimated cut:** ~40% of `guides/margin/liquidation.mdx` becomes a cross-reference.

---

### R2 — Health Factor Formula Duplicated (High)

**Files:** `learn/health-factor.mdx` · `guides/margin/health-factor.mdx`

**What overlaps:**
Both pages state the formula (`Collateral Value / Debt Value`), provide worked examples with numbers, and explain what events make it go up or down. The learn page adds the borrow-check formula and a UI visual; the guides page adds the three metrics bar fields. The definitions are nearly identical.

**Why it is a problem:**  
The worked example tables use different numbers but teach the same thing. New users who read both will feel they are reading the same document twice. If the threshold ever changes from 1.1×, it must be updated in both files.

**Recommended approach:**

| Page | Should contain | Should remove |
|---|---|---|
| `learn/health-factor.mdx` | Formula, borrow-check formula derivation, withdraw-check, all three check types, visual gauge | Generic "what it means" prose — assume the reader already knows what it is |
| `guides/margin/health-factor.mdx` | UI metrics bar fields, practical "what moves it" table for users, monitoring advice | Full formula re-derivation — replace with one line: *"The Health Factor equals your total collateral value divided by your total debt value, both in USD. See [Health Factor in Core Concepts](/learn/health-factor) for the full formula."* |

**Estimated cut:** Remove the formula section and worked examples from `guides/margin/health-factor.mdx` (~30% of the file).

---

### R3 — Lending Pool Mechanics Spread Across Three Files (High)

**Files:** `learn/lending-pools.mdx` · `learn/lending-pool-mechanics.mdx` · `guides/earn/overview.mdx`

**What overlaps:**

| Concept | learn/lending-pools | learn/lending-pool-mechanics | guides/earn/overview |
|---|---|---|---|
| What a lending pool is | ✓ | ✓ | ✓ |
| How interest accrues | ✓ | ✓ | ✓ |
| vToken exchange rate | ✓ | ✓ | ✓ |
| Utilization drives APY | ✓ | ✓ | ✓ |
| Per-asset isolation | ✓ | ✓ | — |

`learn/lending-pools.mdx` and `learn/lending-pool-mechanics.mdx` are particularly close — the latter appears to be a deeper technical version of the former, but there is no clear boundary between them. Both live in `learn/` with no explanation of why they are separate pages.

**Recommended approach:**

1. **Merge** `learn/lending-pools.mdx` and `learn/lending-pool-mechanics.mdx` into a single `learn/lending-pools.mdx` with two clearly separated sections: a conceptual overview up top and a technical mechanics deep-dive below a horizontal rule.
2. **Shorten** the lending pool explanation in `guides/earn/overview.mdx` to two sentences with a link to the merged learn page. The earn overview's job is to tell LPs what to do, not re-teach pool architecture.

**Proposed shortened earn/overview pool explanation:**
```mdx
Vanna's lending pools hold the assets that traders borrow. When you supply, your deposit funds
active borrows, and the interest traders pay flows back to you automatically via the vToken
exchange rate. [See how lending pools work →](/learn/lending-pools)
```

---

### R4 — "Why LP Yields Are Higher" Written Three Times (High)

**Files:** `guides/how-vanna-works.mdx` · `guides/earn/overview.mdx` · `guides/for-liquidity-providers.mdx`

**What overlaps:**
All three pages explain the same structural argument: undercollateralized leverage → higher utilization → higher rates → higher LP yield. `how-vanna-works.mdx` calls it the "Vanna Flywheel." `earn/overview.mdx` calls it "Key factors driving LP earnings." `for-liquidity-providers.mdx` links to the flywheel section.

**Why it is a problem:**  
The flywheel is one of Vanna's core value propositions. Having it fully written out in three places means any refinement of the argument must be applied in three places, and the three versions are already slightly different in emphasis.

**Recommended approach:**  
Establish `guides/how-vanna-works.mdx` as the **single canonical home** of the flywheel explanation. Both other pages should reference it:

```mdx
<!-- guides/earn/overview.mdx — replace the "Key factors" section with: -->
Yields on Vanna are structurally higher than standard DeFi money markets because of how the
protocol's leverage model drives utilization. [See the Vanna Flywheel →](/guides/how-vanna-works#the-vanna-flywheel)
```

```mdx
<!-- guides/for-liquidity-providers.mdx — the existing link is already correct, just remove any inline re-explanation -->
```

---

### R5 — Margin Account Capabilities Listed Twice (Medium)

**Files:** `guides/for-traders.mdx` · `guides/margin/overview.mdx`

**What overlaps:**
Both pages list the same Pro Mode capabilities: open an account, deposit collateral, borrow up to 10×, deploy across strategies, monitor Health Factor. `for-traders.mdx` introduces the concept; `margin/overview.mdx` re-lists it as the page introduction.

**Recommended approach:**  
`guides/for-traders.mdx` should remain an **entry-point page** that routes users to the right guide. The capability list belongs in `margin/overview.mdx`. Shorten `for-traders.mdx` Pro Mode description to two sentences and a card link:

```mdx
## Pro Mode

Pro Mode gives you full control over a Margin Account — your own isolated smart contract for
borrowing, trading, and yield farming in one place.

<Card title="Margin Account Overview" href="/guides/margin/overview" icon="book-open">
  Full breakdown of how the Margin Account system works — collateral, borrowing, health
  factor, and all available actions.
</Card>
```

---

### R6 — vToken Mechanics Re-Explained in Earn Guide (Medium)

**Files:** `learn/vtokens.mdx` · `guides/earn/overview.mdx`

**What overlaps:**
`guides/earn/overview.mdx` explains: you receive vTokens, each vToken grows in value as interest accrues, withdraw by redeeming at the current exchange rate. `learn/vtokens.mdx` covers all of this in depth plus the ERC-4626 share model and the exchange rate formula.

**Recommended approach:**  
The earn overview should explain the **user experience** only ("you receive vTokens that grow in value") and link to `learn/vtokens` for the mechanics. Remove the exchange rate explanation from the earn overview entirely.

---

### R7 — Permissionless Liquidation Explained Twice (Medium)

**Files:** `learn/liquidation.mdx` · `guides/margin/liquidation.mdx`

**What overlaps:**
Both pages explain that anyone can trigger a liquidation, why this is economically correct, and that the liquidator earns a fee. This is a sub-finding of R1 but worth calling out separately because the two versions differ in emphasis — the learn page argues *why* permissionless is the right design; the guides page just states *that* it is permissionless.

**Recommended approach:**  
Keep the "why permissionless" argument in `learn/liquidation.mdx` only. In `guides/margin/liquidation.mdx`, one sentence is sufficient:

```mdx
Liquidation is permissionless — any user or automated system on the network can trigger it,
because the protocol rewards liquidators economically for acting quickly.
[Why this matters →](/learn/liquidation#why-permissionless)
```

---

### R8 — "Borrowed Assets Stay in Margin Account" Repeated (Medium)

**Files:** `guides/margin/borrow.mdx` · `guides/margin/overview.mdx` · `guides/for-traders.mdx`

**What overlaps:**
The constraint that borrowed assets cannot be sent to a wallet — they stay inside the Margin Account and must be deployed via `execute()` — is stated in all three files. This is an important user-facing constraint that warrants mention, but three mentions in documents that users often read sequentially creates a "yes, I know" effect.

**Recommended approach:**  
State it **once** with emphasis in `guides/margin/borrow.mdx` (the most relevant context). Remove from `overview.mdx` and `for-traders.mdx` entirely, or reduce to a one-line reminder in `overview.mdx` only.

---

### R9 — LP Supply Flow Described in Overview and Supply Guide (Low)

**Files:** `guides/earn/overview.mdx` · `guides/earn/supply.mdx`

**What overlaps:**
The overview ends with a card linking to the supply guide, but it also partially describes the supply flow ("supply assets, receive vTokens, earn yield"). The supply guide covers this step-by-step. Minor overlap, but the overview's second paragraph (starting "When you supply assets…") restates what the supply guide already covers in detail.

**Recommended approach:**  
The overview paragraph is acceptable as orientation. Shorten it to three sentences maximum. The detailed "how to supply" content belongs exclusively in `guides/earn/supply.mdx`.

---

### R10 — "1.1× Threshold" Restated in Every Margin Page (Low)

**Files:** `guides/margin/overview.mdx`, `guides/margin/borrow.mdx`, `guides/margin/health-factor.mdx`, `guides/margin/liquidation.mdx`, `guides/margin/transfer-collateral.mdx`, `snippets/risk-notice.mdx`

**What overlaps:**
Every margin-related page states that the Health Factor must stay above 1.1× or liquidation occurs. This is correct behaviour — it is the most important constraint — but the phrasing is re-introduced from scratch each time rather than being stated once and referenced.

**Recommended approach:**  
The `snippets/risk-notice.mdx` already contains this warning and is included at the top of margin pages. Pages that already include the snippet should **not** re-state the 1.1× threshold in their body text unless they are the Health Factor or Liquidation page (where it is definitionally relevant). Other margin pages (borrow, deposit, repay, transfer) can rely on the snippet.

---

### R11 — "How to Avoid Liquidation" Duplicated (Low)

**Files:** `guides/margin/liquidation.mdx` · `guides/margin/health-factor.mdx`

**What overlaps:**
`liquidation.mdx` has a full "How to avoid liquidation" section with five bullet points. `health-factor.mdx` has a "What changes your Health Factor" section covering the same levers (repay debt, add collateral, avoid borrowing to maximum). These are structurally identical in purpose.

**Recommended approach:**  
Keep the full "How to avoid liquidation" section in `guides/margin/liquidation.mdx` — it belongs there. In `health-factor.mdx`, replace the advice section with a two-column table (event → HF direction) and a single cross-reference:

```mdx
For specific steps to take when your Health Factor is declining,
see [How to avoid liquidation](/guides/margin/liquidation#how-to-avoid-liquidation).
```

---

## Consolidation Priority Checklist

```
[ ] R1  — Differentiate learn vs guides liquidation pages; add cross-links
[ ] R2  — Remove formula re-derivation from guides/margin/health-factor.mdx
[ ] R3  — Merge learn/lending-pools + learn/lending-pool-mechanics into one file
[ ] R4  — Make how-vanna-works.mdx the single home of the flywheel; link from others
[ ] R5  — Shorten for-traders.mdx Pro Mode section to 2 sentences + card
[ ] R6  — Remove vToken exchange rate explanation from earn/overview.mdx
[ ] R7  — Collapse permissionless explanation to 1 sentence in guides liquidation
[ ] R8  — Remove "borrowed assets stay in account" from overview and for-traders
[ ] R9  — Cap earn/overview supply flow description at 3 sentences
[ ] R10 — Remove standalone 1.1× re-statements from pages that already use the snippet
[ ] R11 — Replace health-factor advice section with cross-reference to liquidation page
```

---

## Estimated Impact

| Metric | Current state | After consolidation |
|---|---|---|
| Total files with liquidation explanation | 2 full pages | 1 full + 1 referenced |
| Total files with HF formula | 2 full pages | 1 full + 1 summary |
| Total files with lending pool explanation | 3 pages | 2 (merged learn + short earn) |
| Total files with flywheel/yield argument | 3 pages | 1 canonical + 2 links |
| Maintenance surface for threshold changes | 6+ files | 1 snippet + 2 concept pages |

**Overall:** approximately 20–25% reduction in total word count across the User Guide and Core Concepts, with no loss of information — only removal of repetition and consolidation of scattered explanations into single authoritative sources.
