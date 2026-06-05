# Vanna Protocol Docs — Technical Writing Review

> Reviewed by: Senior Web3 Technical Writer & DeFi Protocol Reviewer  
> Date: 2026-06-04  
> Scope: User Guide + Core Concepts (`Vanna_Docs_Final`)

---

## 1. Strengths

- **Consistent component usage** — Mintlify's `<Steps>`, `<Card>`, `<Warning>`, and `<Snippet>` components are used correctly and uniformly throughout.
- **Risk transparency** — Liquidation risk warnings appear at every entry point where users could lose funds. The three snippets (`risk-notice`, `health-factor-warning`, `testnet-notice`) eliminate copy-paste duplication.
- **Two-track content** — Separating User Guides from Core Concepts cleanly serves both non-technical users and technically curious readers.
- **Step-by-step guides** — Supply, Withdraw, Borrow, Repay guides follow a predictable structure that new DeFi users can follow.
- **Analytics depth** — The Risk Explorer and Liquidations pages are unusually thorough for user-facing docs — most protocols skip this entirely.
- **Snippet architecture** — Reusable warning snippets are a strong authoring pattern. Zero drift between instances.

---

## 2. Issues Found

### A. Stale "Perps" References (High Priority)

The perps page was removed, but mentions remain in live content:

| File | Line content | Problem |
|---|---|---|
| `home.mdx` | "deploy capital across spot, **perps**, and yield strategies" | Perps removed — misleads users |
| `guides/for-traders.mdx` | "deploy composably to spot/**perps**/farm" | Same stale reference |

---

### B. "Health Ratio" vs "Health Factor" — Naming Inconsistency (High Priority)

The `learn/` section calls it **Health Ratio**. Every guide, snippet, and UI metric calls it **Health Factor**. These are used as synonyms but never explained as such. A first-time user reading Core Concepts then switching to the User Guide will think these are two different numbers.

| File | Term used |
|---|---|
| `learn/health-ratio.mdx` | Health Ratio |
| `learn/liquidation.mdx` | health ratio |
| All `guides/` files | Health Factor |
| All snippets | Health Factor |

---

### C. Interest Rate Model — Technical Contradiction (High Priority)

`learn/lending-pool-mechanics.mdx` describes a **kinked rate curve** that stays flat below 70% then accelerates. `learn/interest-rate-model.mdx` explicitly says it is a **smooth polynomial curve, not kinked**. One of these is wrong. This directly undermines trust in the Core Concepts section.

---

### D. Leverage Claim Inconsistency (Medium Priority)

| File | Claim |
|---|---|
| `guides/for-traders.mdx` | "borrow up to **10×**" |
| `home.mdx` | "borrow up to **10×**" |
| `learn/composable-leverage.mdx` | "**10×–100×** leverage" |

10× and 100× are not the same. If composable leverage stacking can reach 100×, that needs its own explanation with explicit risk warnings — not a casual mention.

---

### E. Liquidation Fee Floor — Unexplained 0% Case (Medium Priority)

`guides/margin/liquidation.mdx` states the fee ranges from **0% to 15%**. It explains the 15% upper end (large positions) but never explains when a user would face 0%. Users will reasonably ask: "Can I have a position where the liquidator gets nothing? Why would anyone liquidate me then?"

---

### F. Withdraw Page — Misleading "Liquidation Penalties" Language (Medium Priority)

`guides/earn/withdraw.mdx` says LPs get back their deposit plus "**liquidation penalties earned**." The liquidation page says LPs receive a **share of liquidation fees**. "Penalties" implies punitive charges to users — "fees" is the accurate and less alarming term. These should match.

---

### G. Liquidation Flow — Ambiguous Collateral Transfer Step (Medium Priority)

In `guides/margin/liquidation.mdx`, Step 3 says "The protocol transfers your **full collateral** to the liquidator," then Step 4 says "The liquidator repays your full debt." This implies the liquidator receives all collateral before repaying — which may not reflect the actual on-chain atomicity. If the collateral transfer and debt repayment happen in a single atomic transaction, framing them as sequential steps is misleading to technically aware users.

---

### H. Origination Fee — Amount Not Disclosed (Medium Priority)

`guides/margin/borrow.mdx` mentions an origination fee is deducted at borrow time but states no amount. Users about to borrow need to know the cost upfront. The `resources/fees.mdx` page exists but is never linked from the borrow guide.

---

### I. Lite Mode — Navigation Dead End (Low Priority)

`guides/farm/leveraged-yield.mdx` is dedicated to Lite Mode. The `guides/for-traders.mdx` page also describes Lite Mode as a distinct path. However, the sidebar navigation has no "Lite Mode" entry under Farm — the page is only reachable if a user reads For Traders and follows the link. New users going directly to the Farm section will never find it.

---

### J. b-Tokens — Unexplained Acronym (Low Priority)

`guides/farm/blend-pools.mdx` introduces b-Tokens without explaining what "b" stands for (Blend). Users unfamiliar with the Blend Protocol have no context. One line of explanation would resolve this.

---

### K. Redundancy — Liquidation Explained Twice at Similar Depth (Low Priority)

`learn/liquidation.mdx` and `guides/margin/liquidation.mdx` both fully describe the liquidation process. The learn page is more technical (health ratio formula, protocol-level flow). The guides page is user-facing (steps, what to do). The overlap is acceptable in principle, but the two pages have no cross-links explaining "for the technical mechanics, see Core Concepts." Users who land on the wrong one may feel they have the full picture when they don't.

---

## 3. Recommended Improvements

1. **Standardize on "Health Factor"** across all files including `learn/`. Rename `learn/health-ratio.mdx` to `learn/health-factor.mdx` and do a find-replace across the `learn/` folder.

2. **Resolve the kinked vs. smooth rate curve contradiction** — audit the actual contract and pick one description. Add a note in the other page pointing to the authoritative source.

3. **Remove or update all perps references** — replace "spot, perps, and yield strategies" with "spot and yield strategies" in `home.mdx` and `guides/for-traders.mdx`.

4. **Clarify the 100× leverage claim** — either remove it from `learn/composable-leverage.mdx` or add an explicit callout explaining that 100× is a theoretical ceiling under recursive composable loops, not a UI-accessible option.

5. **Explain the 0% liquidation fee case** — add one sentence: at what position size does the fee floor at 0%, or is 0% only for positions where the debt-to-collateral ratio leaves no surplus?

6. **Fix "liquidation penalties" → "liquidation fee distributions"** in the withdraw guide.

7. **Link `resources/fees` from `guides/margin/borrow.mdx`** — add a See Also line: "For the current origination fee rate, see the [Fees](/resources/fees) page."

8. **Add Lite Mode to the Farm sidebar group** in `docs.json`.

9. **Clarify b-Token origin** in `guides/farm/blend-pools.mdx` — one line: "b-Tokens are receipt tokens issued by the Blend Protocol."

10. **Cross-link liquidation pages** — add a note at the bottom of both liquidation pages pointing to the other.

---

## 4. Improved Versions — Rewritten Sections

### 4a. `home.mdx` — Traders & Borrowers card (remove perps)

```mdx
<a href="/guides/margin/overview" className="vanna-audience-card">
  <div className="vanna-card-title">Traders & Borrowers</div>
  <p>Open a Smart Margin Account, borrow up to 10× against your collateral, and deploy capital across spot and yield strategies.</p>
  <div className="vanna-audience-btn">❯</div>
</a>
```

---

### 4b. `guides/margin/liquidation.mdx` — Liquidation fee section (clarify fee distribution + fix 0% case)

```mdx
## The liquidation fee

The liquidation fee is dynamic and ranges from 0% to 15%, depending on the size of your position. The protocol's `liquidation_fee` parameter determines the exact rate applied at the time of liquidation. The current fee applicable to your position is shown in the Margin metrics bar — check it when you open or manage a position.

Larger positions incur higher fees. This structure ensures that liquidators are always sufficiently incentivized to act quickly on larger, harder-to-close positions, keeping the protocol solvent. Very small positions may incur a fee near 0% because the collateral surplus after debt repayment is too small to warrant a meaningful incentive.

A portion of the liquidation fee is distributed to liquidity providers who have supplied assets to Vanna's lending pools. This means that users who provide liquidity to the protocol benefit not only from lending interest but also from liquidation activity — earning a share of the fees generated each time an undercollateralized position is closed.
```

---

### 4c. `guides/margin/liquidation.mdx` — Steps 3 & 4 (clarify atomic flow)

**Current (ambiguous):**
> Step 3: The protocol transfers your collateral to the liquidator.  
> Step 4: The liquidator repays your full debt.

**Rewritten:**

```mdx
<Step title="The protocol atomically repays your debt and compensates the liquidator">
  In a single transaction, the protocol uses your collateral to repay your full outstanding
  debt to the lending pool. The liquidation fee is simultaneously deducted from your collateral
  and transferred to the liquidator as compensation. These steps happen atomically — there is
  no intermediate state where the liquidator holds your collateral without repaying the debt.
</Step>
```

---

### 4d. `guides/earn/withdraw.mdx` — Fix "liquidation penalties" language

**Find:** `liquidation penalties earned`  
**Replace with:** `liquidation fee distributions earned`

---

### 4e. `guides/farm/blend-pools.mdx` — Add b-Token explanation

Add this line at first mention of b-Tokens:

```mdx
When you supply assets to a Blend pool, you receive **b-Tokens** — receipt tokens issued by the
[Blend Protocol](https://blend.capital) that represent your share of the pool.
```

---

### 4f. `learn/health-ratio.mdx` — Recommended title and opening fix

```mdx
---
title: "Health Factor"
description: "How Vanna calculates position health and when liquidation is triggered."
---
```

Replace all instances of "Health Ratio" in this file with "Health Factor" for consistency with the rest of the docs and the app UI.

---

## Summary

The docs are well-structured and above average for a DeFi protocol. The main work needed is:

| Priority | Fix |
|---|---|
| High | Standardize "Health Factor" across all `learn/` files |
| High | Resolve kinked vs. smooth rate curve contradiction |
| High | Remove stale perps references from `home.mdx` and `for-traders.mdx` |
| Medium | Clarify 0% liquidation fee case |
| Medium | Fix "liquidation penalties" → "liquidation fee distributions" |
| Medium | Fix ambiguous atomic liquidation flow in Steps 3 & 4 |
| Medium | Link fees page from borrow guide |
| Low | Add Lite Mode to Farm sidebar |
| Low | Explain b-Token acronym |
| Low | Cross-link the two liquidation pages |

None of these require new pages — they are all targeted edits to existing content.
