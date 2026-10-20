# Buoyancy Test Scorecard Template

**Framework version**: v0.4

**Target**: [repo URL or product description]  
**Type**: App / Infra / Router-Optimizer  
**Stage**: Prototype / Early product / Scaled product  
**Mode**: Builder / Buyer  
**Date**: [Assessment date]

---

## Verdict

**[Quadrant]** (S=X/10, M=X/2, H=X/10, B=X.X)

[One-paragraph summary: quadrant, type, key strengths/weaknesses, which repo was scored]

---

## Scaffolding (S) = X/10

### 1. Delete test: X/2
**[<10% / 10-40% / >40% deletable]**

[Analysis of code that would be obsolete with a 2x better model]

**Evidence**: [cite paths, line numbers, commit SHAs]

### 2. Prompt sediment: X/2
**[Few / Dozens / Hundreds]**

[Analysis of defensive instructions]

**Evidence**: [cite grep counts, prompt file analysis]

### 3. Dependent chain depth: X/2
**[1-2 / 3-5 / 6+ hops]**

[Analysis of sequential dependent model calls]

**Evidence**: [cite orchestration code, call graphs]

### 4. Swap cost: X/2
**Same-provider upgrade: X, cross-provider switch: X**

[Analysis of model upgrade and switching costs]

**Evidence**: [cite config locations, model reference counts]

### 5. Dependency brittleness: X/2
**[None / Some with tests / Core function depends]**

[Analysis of undocumented vendor dependencies]

**Evidence**: [cite integration code, test coverage]

---

## Measurability (M) = X/2

**[Evals / Tests / None]**

[Analysis of ability to prove model improvements]

**Evidence**: [cite eval framework, test suites]

---

## Hull (H) = X/10

### 6. Proprietary/compounding context: X/2 (confidence: high/med/low)
**[None / Some / Compounds over time]**

[Analysis of proprietary data and context]

**Evidence**: [cite data storage, user context systems]

### 7. Sherlock risk: X/2 (confidence: high/med/low)
**[Easily / Partly / Not without assets]**

[Analysis of whether labs/platforms could replicate as a feature]

**Evidence**: [cite competing features, lab roadmaps]

### 8. Distribution: X/2 (confidence: high/med/low)
**[None / Weak / Strong]**

[Analysis of owned channels and customer relationships]

**Evidence**: [cite verified traction: GitHub stars, package downloads, press]

### 9. Workflow embedding: X/2 (confidence: high/med/low)
**[Standalone / Some integrations / System of record]**

[Analysis of workflow integration depth]

**Evidence**: [cite integration points, system-of-record status]

### 10. Problem ownership: X/2 (confidence: high/med/low)
**[Task / Workflow / Outcome]**

[Analysis of problem ownership level]

**Evidence**: [cite product scope, outcome metrics]

---

## Normalized Score

**B = H/10 - S/10 = X/10 - X/10 = X.X**

**Quadrant**: [Buoyant / Anchored / Dissolvable / Sinking]

**Borderline flags**: [if H or S within 1 point of threshold]

**Gut check**: [Greed or dread when the next model drops?]

**Trend**: [if previous scores exist, compare]

---

## Recommendations

### Delete or simplify (reduce S):
1. [Concrete action to reduce scaffolding]
2. [Concrete action to reduce scaffolding]
3. [Concrete action to reduce scaffolding]
4. [Optional: 2 more actions]
5. [Optional]

### Build a hull (increase H):
1. [Concrete action to build defensibility]
2. [Concrete action to build defensibility]
3. [Concrete action to build defensibility]

---

## Caveats

- **Stage**: [prototype / early / scaled - adjust hull expectations]
- **Short history**: [flag if <20 commits or <3 months]
- **Unverified traction**: [flag self-reported metrics]
- **Other**: [assessment-specific notes]
