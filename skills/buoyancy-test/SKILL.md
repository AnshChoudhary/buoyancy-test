---
name: Buoyancy test
description: >-
  When the user wants to assess whether an AI product, startup, project, or
  GitHub repo is "buoyant" (improves automatically as models improve) using the
  plumbing/scaffolding/hull framework.
---
# Buoyancy test v0.4
Based on Rajiv Ayyangar's "Buoyancy" essay. Scaffolding = code/prompts compensating for weaknesses of models or of platforms the product depends on (depreciates). Plumbing = connecting the model to the problem (appreciates). Hull = assets models don't acquire by getting smarter (data, user context, distribution, workflow embedding).

## 0. Intake
- Target: repo URL, product URL, or description. Mode: Builder (own project) or Buyer (evaluating a vendor).
- Type: App (calls models to do the job), Infra (runs/connects/observes agents or models), or Router/Optimizer (chooses or tunes models). Type changes how questions 1 and 4 are read (see notes).
- Stage: prototype / early product / scaled product. Sets hull expectations; flag, don't punish, a prototype.
- Pick the product repo: when the company page, GitHub org, and founder accounts point to different repos, choose the one matching the current pitch with recent activity; note archived/frozen/experimental repos and say which you scored.

## 1. Pass 1: evidence
Repos: clone read-only; find prompt files, model-call sites, config, tests/evals, integrations with other vendors' tools, git log. Cite paths, line numbers, counts, commits for every score. Short history (<~20 commits or <3 months): flag "insufficient history" and skip trend signals. Rule-word counts (never/always/must/IMPORTANT) are rough; read a sample of hits, and include docs that encode rules.
Buyers: docs, changelog, pricing, model disclosures, release notes around past model launches.

## 2. Scaffolding S (0-2 each, max 10, lower is better)
1. Delete test: share of prompt/orchestration code deletable if a 2x better model shipped tomorrow. <10%=0, 10-40%=1, >40%=2. Count JSON repair, re-asks, reviewer models, guardrails, output fixers. Separate the build process from durable output: if scaffolding produces artifacts that keep working without a model (generated code, integrations), score only the scaffolding, and note the artifact as possible hull.
2. Prompt sediment: defensive instructions. Few=0, dozens=1, hundreds or nobody knows why=2.
3. Dependent chain depth: count only sequential dependent model hops (parallel independent calls count as one). 1-2=0, 3-5=1, 6+=2.
4. Swap cost, scored as the max of (a) same-provider upgrade (config=0, retune=1, migration=2) and (b) cross-provider switch weighted half, rounded down. Report both. For Router/Optimizer products, a per-release model catalog update is plumbing, not swap cost; score only whether it's centralized (one source=0, scattered across many files=1).
5. Dependency brittleness: reliance on undocumented behavior of other vendors' products (screen scraping, text matching of UIs or outputs, private APIs). None=0, some with tests=1, core function depends on it=2. Especially important for Infra.

## 3. Measurability M (0-2, reported separately)
Can you prove a new model is better within a day? Evals comparing model versions=2, tests or spot checks=1, none=0. Low M means S is unverified.

## 4. Hull H (0-2 each, max 10, higher is better). Every score needs a confidence label (high/med/low).
6. Proprietary/compounding context: none=0, some=1, compounds per user over time=2.
7. Sherlock risk: could a frontier lab, agent vendor, or platform ship it as a feature within 12 months? Name the specific overlapping feature if one exists. Easily=0, partly=1, not without your assets=2.
8. Distribution: owned channel or customer relationship. None=0, weak=1, strong=2. Verify traction (GitHub API stars, package downloads from npm/PyPI/crates, press) instead of quoting the company; self-reported only = cap confidence at low.
9. Workflow embedding: standalone=0, some integrations=1, system of record or deep integration=2.
10. Problem ownership: task=0, workflow=1, outcome=2.

## 5. Pass 2: fill the hull gaps
For low-confidence hull scores, ask the user/founder short questions (one at a time) or research the market, competitors, and lab roadmaps on the web. Update scores and confidence.

## 6. Result
- Normalize: B = H/10 - S/10 (range -1 to +1).
- Quadrant: Buoyant (H>=6, S<=4), Anchored (H>=6, S>=5), Dissolvable (H<=5, S<=4), Sinking (H<=5, S>=5). Mark borderline when within 1 point of a threshold.
- Gut check: "Greed or dread when the next model drops?"
- Trend: compare to earlier scores if any. Rising B after a release = floating.

## 7. Report
One-paragraph verdict with quadrant and type, which repo was scored, then a score table with evidence and confidence, M, 3-5 "delete or simplify" actions, 2-3 "build a hull" actions, and caveats (stage, short history, unverified traction). Never modify the repo or post anything.
