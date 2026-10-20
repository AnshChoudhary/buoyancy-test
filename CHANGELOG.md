# Buoyancy Test Changelog

## Known Issues / v0.5 Roadmap

The following issues have been identified and will be addressed in v0.5:

### 1. Precise Borderline Rule
**Issue**: Current rule ("within 1 point of a threshold") is ambiguous when a 1-point change flips the quadrant vs when it doesn't.

**Example**: H=5, S=4 (Dissolvable) is borderline—either H+1 → Buoyant or S+1 → Sinking. But H=4, S=3 (Dissolvable) is *not* borderline—both would need to change by 2+ points.

**v0.5 fix**: Mark borderline only when *one* score is exactly on a threshold (H=5, H=6, S=4, or S=5) and the other is near the boundary.

### 2. Buyer-Mode Scoring Improvements
**Issue**: Buyer mode (public info only) currently estimates scaffolding (S) from observed behavior, sets M=0, and scores hull (H) normally. This creates inconsistent confidence across dimensions.

**v0.5 fix**: 
- Add a short **scaffolding checklist for buyer mode**: observable indicators like multi-agent chains mentioned in docs, retry logic in demos, changelog references to prompt tuning.
- Report buyer-mode S as a range (e.g., "S=3-5 estimated") rather than a point estimate.
- Add a "buyer mode" flag to the result to make the assessment's limitations explicit.

### 3. Better Models Question
**Issue**: The delete test (Q1) asks "what code becomes obsolete with a 2x better model?" This is too narrow—some products *grow* their job when models improve (outcome-based products expand scope), while others shrink (task products get commoditized).

**v0.5 addition**: Add a question to Q1 rationale: "When models improve, does your product's job grow or shrink?" Greed if grow, dread if shrink.

### 4. Confidence Caps for Closed-Source Core
**Issue**: Some products are partly open-source (docs, SDKs) but keep core orchestration code private. Current guidance doesn't cap confidence consistently for these cases.

**v0.5 rule**: When core model-call orchestration code isn't public:
- Cap scaffolding (S) confidence at **medium** max
- Cap measurability (M) confidence at **low** max (unless evals are public)
- Hull (H) confidence can still be high if traction is verifiable

### 5. Exclude Logs and Fixtures from Counts
**Issue**: Prompt sediment (Q2) and rule-word counts can be inflated by test fixtures, example data, and logged outputs that aren't actual prompts sent to models.

**v0.5 fix**: Explicitly exclude from counts:
- Test fixtures and mock data
- Logged/archived model outputs
- Documentation examples (unless they're templates loaded into production prompts)

Count only: production prompt files, system messages, and orchestration code that generates prompts.

---

## v0.4 (Current)

**Major changes**:
- Added **product type** (App / Infra / Router-Optimizer) to intake, changes interpretation of Q1 and Q4
- Added **dependency brittleness** as Q5 (scaffolding), especially important for Infra products
- Split **delete test** (Q1) to separate build process from durable output (generated code, integrations)
- Refined **swap cost** (Q4) with special Router/Optimizer rule: per-release model catalog updates are plumbing, not scaffolding; score only centralization
- Added **verified traction** requirement for Q8 (distribution): use GitHub API, npm/PyPI/crates downloads, or press; self-reported only = cap confidence at low
- Added **borderline flags** when H or S within 1 point of thresholds (e.g., H=5, H=7, S=4, S=5)
- Added **product-repo pick** guidance: when orgs have multiple repos, choose the one matching current pitch with recent activity
- Added **short-history flag** (<~20 commits or <3 months): skip trend signals, note insufficient history
- Refined **Sherlock risk** (Q7) to explicitly ask for overlapping feature names when they exist
- Scaffolding scale now **0-10** across 5 questions (Q1-Q5, each 0-2)
- Hull scale remains **0-10** across 5 questions (Q6-Q10, each 0-2)

**Rationale**: v0.3 mixed different concerns (measurability in scaffolding, swap cost unclear for routers, traction unverified). v0.4 separates measurability, adds product-type context, and sharpens evidence requirements.

---

## v0.3

**Major changes**:
- Split **measurability** (M) out as a separate 0-2 score, no longer part of scaffolding
- Refined **dependent chain depth** (Q3) to count only sequential dependent hops; parallel independent calls count as one
- Split **swap cost** (Q4) into same-provider upgrade vs cross-provider switch, scored as max(a, b/2 rounded down)
- Added **confidence labels** (high/med/low) required for all hull scores
- Added **stage** to intake (prototype / early product / scaled product) to set hull expectations
- Added **short-history flag** guidance (<~20 commits or <3 months)
- Scaffolding scale: **0-8** (8 questions, each 0-1, with Q1 worth 0-2)
- Hull scale: **0-10** (5 questions, each 0-2)
- B = H/10 - S/8

**Rationale**: v0.2 conflated "can you measure it?" with scaffolding. Measurability is orthogonal—you can have clean code (low S) but no evals (low M), or messy code (high S) with great evals (high M). Splitting M out clarifies that low M means S is unverified.

---

## v0.2

**Major changes**:
- Expanded to **10 questions** (scaffolding + hull), each scored 0-2
- Introduced **quadrants**: Buoyant, Anchored, Dissolvable, Sinking
- Added **normalized score** B = H/10 - S/10 (range -1 to +1)
- Scaffolding scale: **0-10**
- Hull scale: **0-10**
- Defined thresholds: Buoyant (H≥6, S≤4), Anchored (H≥6, S≥5), Dissolvable (H≤5, S≤4), Sinking (H≤5, S≥5)

**Rationale**: v0.1's qualitative questions lacked consistent scoring. v0.2 introduced numeric scales and quadrants to make assessments comparable across products.

---

## v0.1 (Initial)

**Initial framework**:
- Qualitative questions only, no numeric scores
- Core concepts: plumbing (appreciates), scaffolding (depreciates), hull (defensibility)
- Questions covered:
  - Prompt complexity
  - Chain length
  - Error handling
  - Model swapping
  - Data moat
  - Distribution
  - Workflow embedding

**Rationale**: Codified Rajiv Ayyangar's essay into a structured assessment. Focused on "greed or dread when the next model drops?"

---

## Framework Origins

Based on Rajiv Ayyangar's essay ["Buoyancy"](https://blog.rajivayyangar.com/buoyancy/).

**Key insight**: AI products built on scaffolding (compensating for model weaknesses) depreciate as models improve. Products built on plumbing (connecting models to problems) appreciate. Hull (defensibility) determines long-term survival.
