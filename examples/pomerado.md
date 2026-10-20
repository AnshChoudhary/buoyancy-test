# Pomerado Buoyancy Assessment

**Framework version**: v0.4

**Target**: [github.com/Pomerado/pomerado](https://github.com/Pomerado/pomerado) (YC F26)  
**Type**: App  
**Stage**: Early product  
**Date**: October 2026

## Verdict

**Sinking (borderline)** (S=8/10, M=1/2, H=5/10, B=-0.3)

Pomerado is a multi-agent system with a minter + guardian reviewer chain. Heavy scaffolding: ~4,131 prompt lines with 444 rule-word lines (never/always/must/IMPORTANT), retry and recovery layers, complex orchestration. High dependency brittleness from browser automation and site-driving. Measurability is low (only basic tests). Borderline on hull (H=5, just below Anchored threshold)—durable deterministic integrations (crypto wallets, smart contracts) offer some defensibility, but not yet a system of record.

---

## Scaffolding (S) = 8/10

### 1. Delete test: 2/2
**>40% deletable**

Multi-agent chain with minter + guardian reviewer. Reviewer model checks minter output, catches errors, re-asks. If a 2x better model shipped, the guardian layer could be deleted or simplified.

**Evidence**: Explicit reviewer agent, error-checking loops.

### 2. Prompt sediment: 2/2
**Hundreds of instructions**

~4,131 prompt lines across the codebase, including 444 lines with rule-words (never/always/must/IMPORTANT). Heavy defensive instructions accumulated over development.

**Evidence**: Prompt file line counts, rule-word grep hits.

### 3. Dependent chain depth: 1/2
**3-5 hops**

Minter → Guardian → (retry if guardian rejects) → output. Sequential dependent chain.

**Evidence**: Multi-agent sequential orchestration.

### 4. Swap cost: 1/2
**Re-tuning required**

Prompts are tuned to current model behavior. Upgrading or switching models would require re-tuning the minter and guardian prompts.

**Evidence**: Heavy prompt customization (4,131 lines) implies high tuning investment.

### 5. Dependency brittleness: 2/2
**Core function depends on it**

Browser automation for driving third-party crypto sites and DeFi protocols. Reverse-engineered site structures, DOM element selectors, button text matching. Breaks when sites redesign, add anti-bot measures, or change interaction flows. Core to the product's ability to execute cross-chain and cross-protocol operations.

**Evidence**: Browser automation code, site-specific selectors, fragile integration patterns.

---

## Measurability (M) = 1/2

**Tests or spot checks**

Basic tests exist, but no comprehensive evals comparing model versions. Difficult to prove whether guardian layer is necessary or whether upgrading models would reduce error rates.

**Evidence**: Test directory present, but no model-version eval suite.

---

## Hull (H) = 5/10

### 6. Proprietary/compounding context: 1/2 (confidence: medium)
**Some proprietary context**

User wallet addresses, transaction history, possibly smart contract state. Could compound over time as users build more integrations.

**Evidence**: Wallet connections imply stored user context.

### 7. Sherlock risk: 1/2 (confidence: medium)
**Partly**

Crypto agent frameworks are emerging (Coinbase agents, Paradigm tooling). Labs could ship basic crypto agents within 12 months. However, Pomerado's deterministic integrations (if built) are harder to replicate.

**Evidence**: Competitive landscape includes other crypto agent platforms.

### 8. Distribution: 1/2 (confidence: low - unverified)
**Weak**

YC F26 batch (verified), but early-stage traction unclear. No public download counts or verified user numbers.

**Evidence**: YC affiliation (high confidence), but other metrics unverified.

### 9. Workflow embedding: 2/2 (confidence: medium)
**Deep integration**

Connects to crypto wallets, DEX/DeFi protocols, manages multi-chain transactions. Becoming the execution layer for crypto operations. Users rely on Pomerado to orchestrate complex DeFi workflows they couldn't easily replicate manually.

**Evidence**: Wallet integrations, multi-protocol orchestration, transaction management.

### 10. Problem ownership: 0/2 (confidence: medium)
**Task**

Executes crypto tasks (minting, swapping), but doesn't own outcomes (portfolio performance, risk management, DeFi strategy).

**Evidence**: Agent performs tasks, doesn't guarantee financial outcomes.

---

## Normalized Score

**B = H/10 - S/10 = 5/10 - 8/10 = -0.3**

**Quadrant**: Sinking (H ≤ 5, S ≥ 5)

**Borderline flag**: H=5 exactly (Anchored threshold). A 1-point improvement in hull (H=6) would shift to Anchored.

**Gut check**: Dread. If GPT-5 drops, those 4,131 prompt lines will need re-tuning, the guardian layer may become obsolete, the retry logic may break, and the browser automation will still be fragile. High maintenance burden as models improve.

---

## Recommendations

### Delete or simplify (reduce S):
1. **Fix dependency brittleness first**: Replace browser automation with documented APIs wherever possible. This is the highest risk (Q5: 2→0 or 1).
2. **Delete guardian layer experiment**: Test whether a better base model (GPT-4 → GPT-5 or Claude 3.5) eliminates the need for the reviewer agent. If so, cut the entire guardian chain (Q1: 2→1).
3. **Prompt consolidation**: Reduce 4,131 prompt lines by 50%. Remove defensive instructions that are no longer necessary (Q2: 2→1).
4. **Simplify retry logic**: If evals show retry loops don't improve success rates, remove them (Q1: further reduction).
5. **Centralize model config**: Move model selection to environment variables or central config to reduce swap cost (Q4: 1→0).

### Build a hull (increase H):
1. **Transaction history as moat**: Store user transaction history, portfolio performance, and learned DeFi strategies (Q6: 1→2).
2. **Outcome ownership**: Offer "portfolio outcome guarantees" or "DeFi strategy optimization" (Q10: 0→1 or 2).
3. **Become the system of record**: Store authoritative transaction state, smart contract interactions, and portfolio analytics. Make Pomerado the source of truth users can't leave (Q9: stays 2, but strengthens).

---

## Caveats

- **Stage**: Early product, so hull scores are lower than they might be at scale
- **Borderline**: H=5 exactly (Sinking/Anchored threshold). Small improvements could shift to Anchored.
- **Dependency brittleness dominates**: S=8 is driven heavily by Q5 (browser automation fragility). This is the most urgent risk.
- **Traction unverified**: Distribution score capped at low confidence due to self-reported metrics
