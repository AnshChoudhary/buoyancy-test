# Caveman Buoyancy Assessment

**Framework version**: v0.4

**Target**: [github.com/JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) (YC F26)  
**Type**: Router/Optimizer (with compression features)  
**Stage**: Early product  
**Date**: October 2026

## Verdict

**Anchored (borderline)** (S=5/10, M=2/2, H=6/10, B=+0.1)

Caveman is a routing + compression platform for AI agents. Router + evals are buoyant (plumbing that gets better with new models). However, compression/voice features bet against cheaper and less verbose models—scaffolding that depreciates if GPT-5 is concise by default. Model IDs scattered across 95 files increase swap cost. Strong measurability (M=2) and decent hull (workflow embedding, some distribution). Borderline between Dissolvable (if S reduced) and Anchored.

---

## Scaffolding (S) = 5/10

### 1. Delete test: 1/2
**10-40% deletable**

Compression and verbosity-reduction layers could become obsolete if GPT-5 ships with native conciseness and lower token costs. Router logic is plumbing (appreciates), but compression is scaffolding (depreciates).

**Evidence**: Compression features compensate for current model verbosity and cost.

### 2. Prompt sediment: 1/2
**Dozens of instructions**

Some defensive prompts around compression quality and voice tuning. Not hundreds, but accumulated over development.

**Evidence**: Prompt files with compression/voice tuning rules.

### 3. Dependent chain depth: 0/2
**1-2 hops**

Router → model selection → single call. No long dependent chains.

**Evidence**: Routing layer adds one hop, then standard model call.

### 4. Swap cost: 2/2
**Migration project (cross-provider switch)**

Model IDs scattered across 95 files. Updating the model catalog (adding GPT-5, Claude 4) requires touching many files. For Caveman as a Router/Optimizer, this is scored as catalog management:

- **Centralized catalog**: 0 (one source)
- **Scattered**: 1 (many files)

However, cross-provider switching (changing underlying providers) is also hard due to scattered config.

**Score**: max(0, 2/2 rounded down) = max(0, 1) = 1

However, under v0.4's Router/Optimizer rule, per-release model catalog updates are plumbing. The scattered references are a catalog management problem: **1/2** for scattered catalog (vs 0 for centralized).

**Evidence**: Model references in 95+ files, but catalog updates (adding GPT-5) are routine.

### 5. Dependency brittleness: 1/2
**Some, with tests**

Routing logic depends on provider API response formats. If providers change error codes or metadata fields, routing may break. Likely has tests to catch this.

**Evidence**: Multi-provider integration requires parsing diverse API responses.

---

## Measurability (M) = 2/2

**Evals comparing model versions**

Has eval framework for testing routing decisions and compression quality across model versions. Can prove whether a new model improves results within a day.

**Evidence**: Eval suite present (high confidence from v0.3 assessment).

---

## Hull (H) = 6/10

### 6. Proprietary/compounding context: 1/2 (confidence: medium)
**Some proprietary context**

User routing preferences, cost/quality trade-offs, possibly learned routing rules per user. Could compound over time as the system learns which models work best for which tasks.

**Evidence**: Routing decisions imply stored user preferences.

### 7. Sherlock risk: 1/2 (confidence: medium)
**Partly**

Labs could ship basic routing (GPT-4o-mini vs GPT-4o) within 12 months. OpenAI's model picker, Anthropic's Claude Haiku/Sonnet/Opus tiers are primitive routers.

However, cross-provider routing (OpenAI vs Anthropic vs Google) and learned user preferences are harder to replicate.

**Evidence**: Simple routing exists in ChatGPT (model picker), but Caveman's cross-provider + learning layer is defensible.

### 8. Distribution: 1/2 (confidence: low - unverified)
**Weak**

YC F26 batch (verified), but early-stage traction unclear. No public download counts or verified user numbers.

**Evidence**: YC affiliation (high confidence), other metrics unverified.

### 9. Workflow embedding: 2/2 (confidence: medium)
**Deep integration**

Sits in the middle of agent workflows—every model call goes through Caveman. Becomes part of the agent infrastructure. Switching costs increase as more agents integrate.

**Evidence**: Routing layer = required for agent operation.

### 10. Problem ownership: 1/2 (confidence: medium)
**Workflow**

Owns the "model selection" workflow, not the outcome (task success, cost optimization guarantees).

**Evidence**: Routing is a workflow step, not end-to-end outcome ownership.

---

## Normalized Score

**B = H/10 - S/10 = 6/10 - 5/10 = +0.1**

**Quadrant**: Anchored (H ≥ 6, S ≥ 5)

**Borderline flags**: H=6 exactly (Buoyant/Anchored threshold), S=5 exactly (Dissolvable/Anchored threshold). A 1-point reduction in S (S=4) would shift to Buoyant. A 1-point reduction in H (H=5) would shift to Dissolvable.

**Gut check**: Mixed. Greed for routing (GPT-5 makes routing decisions better), dread for compression (GPT-5 might obsolete verbosity-reduction features).

---

## Recommendations

### Delete or simplify (reduce S):
1. **Centralize model catalog**: Move model IDs from 95 files to a single config or database (Q4: 2→0 for v0.4 Router rule).
2. **Compression deprecation plan**: Monitor model verbosity trends. If GPT-5 is concise by default, sunset compression features proactively (Q1: 1→0).
3. **Provider API abstraction**: Centralize provider response parsing to reduce dependency brittleness (Q5: 1→0).

### Build a hull (increase H):
1. **Learned routing**: Store which models work best for which tasks per user, compounding over time (Q6: 1→2).
2. **Cost optimization guarantees**: Offer outcome-based pricing (we guarantee X% cost savings) to move from workflow to outcome ownership (Q10: 1→2).
3. **Agent marketplace**: Build a directory of agents optimized for Caveman routing, locking in distribution (Q8: 1→2).

---

## Caveats

- **Stage**: Early product, so hull scores could rise as distribution and context accumulate
- **Borderline**: Both H=6 and S=5 exactly (thresholds). Small changes could shift quadrant: S-1 → Buoyant, H-1 → Dissolvable.
- **Compression risk**: Betting against model improvement (cheaper, more concise models) is risky. Monitor closely.
- **Catalog management**: Under v0.4 Router rule, scattered model IDs are scored as catalog management (1/2), not full swap cost (2/2).
