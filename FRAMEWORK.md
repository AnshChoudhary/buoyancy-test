# Buoyancy Test Framework v0.4

Complete rubric with rationale for assessing whether AI products are buoyant.

Based on Rajiv Ayyangar's ["Buoyancy"](https://blog.rajivayyangar.com/buoyancy/) essay.

## Core Concepts

**Scaffolding** = Code and prompts compensating for weaknesses of current models or platforms the product depends on. Depreciates as models improve.

**Plumbing** = Code that connects the model to the problem. Appreciates as models get better.

**Hull** = Assets models don't acquire just by getting smarter: data, user context, distribution, workflow embedding, problem ownership.

## 0. Intake

**Purpose**: Establish what you're evaluating and set appropriate expectations.

### Target
- Repo URL, product URL, or description
- Mode: **Builder** (own project) or **Buyer** (evaluating a vendor)

### Type
Choose one:
- **App**: Calls models to do the job (writing, coding, analysis, etc.)
- **Infra**: Runs, connects, or observes agents or models (runtime, gateway, monitoring)
- **Router/Optimizer**: Chooses or tunes models (routing layer, optimization platform)

*Rationale*: Type changes how questions 1 and 4 are interpreted. Infra products have different swap costs than apps. Routers score model catalog updates as plumbing, not scaffolding.

### Stage
- Prototype / Early product / Scaled product

*Rationale*: Sets hull expectations. A prototype may have zero distribution or workflow embedding; flag this rather than penalizing early-stage builders.

### Pick the Product Repo
When a company page, GitHub org, and founder accounts point to different repositories, choose the one matching the current pitch with recent activity. Note archived, frozen, or experimental repos, and say which you scored.

*Rationale*: Companies pivot. Score what they're actually building now, not legacy code.

## 1. Pass 1: Evidence Collection

### For Repositories
Clone read-only. Find:
- Prompt files and model-call sites
- Config files
- Tests and evals
- Integrations with other vendors' tools
- Git log and commit history

**Cite paths, line numbers, counts, and commits for every score.**

### Short History Flag
If <~20 commits or <3 months of history: flag "insufficient history" and skip trend signals.

*Rationale*: You can't assess technical debt accumulation or track B over time with too little data.

### For Buyers
Review:
- Documentation and changelogs
- Pricing pages
- Model disclosures
- Release notes around past model launches

### Rule-Word Counts
When counting defensive instructions (question 2), search for "never", "always", "must", "IMPORTANT", etc. These counts are rough—read a sample of hits and include docs that encode rules, not just code comments.

*Rationale*: Prompt sediment shows up as repeated defensive instructions. Some are necessary (safety, compliance), but hundreds of arbitrary constraints suggest the team is patching model behavior instead of using better models or clearer prompts.

## 2. Scaffolding (S)

Scored 0-2 each, max 10. **Lower is better.**

### Question 1: Delete Test
**How much of your prompt/orchestration code could you delete if a 2x better model shipped tomorrow?**

- **0**: <10% deletable
- **1**: 10-40% deletable
- **2**: >40% deletable

Count:
- JSON repair and re-ask loops
- Reviewer models that check output
- Guardrails and output fixers
- Retry and recovery layers

**Separate the build process from durable output**: If scaffolding produces artifacts that keep working without a model (generated code, integrations, compiled assets), score only the scaffolding itself. Note the artifact as possible hull (question 6).

*Rationale*: The delete test is the purest measure of scaffolding. If a 10x better model would make half your code obsolete, you're building on depreciating ground.

### Question 2: Prompt Sediment
**How many defensive instructions have accumulated in your prompts?**

- **0**: Few (dozens of words or less)
- **1**: Dozens of instructions
- **2**: Hundreds, or "nobody knows why we added this"

*Rationale*: "Never use emojis unless the user asks" and 200 more rules = brittle prompts tuned to a specific model's failure modes. The next model won't have the same failure modes.

### Question 3: Dependent Chain Depth
**How many sequential, dependent model hops does a typical request make?**

Count only sequential dependent calls. Parallel independent calls count as one.

- **0**: 1-2 hops
- **1**: 3-5 hops
- **2**: 6+ hops

*Rationale*: Every dependent hop compounds latency and error rates. Parallelizable calls are plumbing; long dependent chains are usually scaffolding around weak reasoning.

### Question 4: Swap Cost
**How hard is it to upgrade models or switch providers?**

Calculate **(a) same-provider upgrade** and **(b) cross-provider switch** separately, then score as the max of (a) and (b/2, rounded down). Report both.

Same-provider upgrade:
- **0**: Config change only
- **1**: Re-tuning or prompt adjustments
- **2**: Migration project (re-architecture, data changes)

Cross-provider switch (weighted half):
- **0**: Config change (× 0.5 = 0)
- **1**: Re-tuning (× 0.5 = 0.5 → rounds to 0)
- **2**: Migration project (× 0.5 = 1)

**Special case for Router/Optimizer products**: A per-release model catalog update (adding GPT-5 to the routing table) is plumbing, not swap cost. Score only whether the catalog is centralized:
- **0**: One source (central config or database)
- **1**: Scattered across many files

*Rationale*: High swap cost traps you on a depreciating platform. If upgrading from GPT-4 to GPT-5 requires re-writing prompts in 47 files, you won't upgrade, and you won't benefit from model improvements.

### Question 5: Dependency Brittleness
**Do you rely on undocumented behavior of other vendors' products?**

Examples: screen scraping, text matching of UI outputs, private APIs, reverse-engineered formats.

- **0**: None, or only documented APIs
- **1**: Some, but with tests and fallbacks
- **2**: Core function depends on it

*Rationale*: Especially important for Infra products. If you parse the Claude Desktop UI by looking for specific button text, your product breaks when Anthropic redesigns the UI.

## 3. Measurability (M)

Scored 0-2, reported separately. Not included in B calculation.

**Can you prove a new model is better within a day?**

- **2**: Evals comparing model versions
- **1**: Tests or spot checks
- **0**: None

*Rationale*: Low M means S is unverified. You might think you need that retry loop, but if you don't have evals, you don't actually know whether it helps. High M enables you to delete scaffolding confidently.

## 4. Hull (H)

Scored 0-2 each, max 10. **Higher is better.** Every score requires a **confidence label** (high/med/low).

### Question 6: Proprietary/Compounding Context
**Do you have data or context models can't get elsewhere?**

- **0**: None (all public data, stateless)
- **1**: Some proprietary context (API keys, user preferences, company data)
- **2**: Compounds per user over time (history, learned preferences, evolving corpus)

*Rationale*: GPT-5 won't have your user's Slack history, internal docs, or past decisions. Context that compounds over time creates switching costs for your users.

### Question 7: Sherlock Risk
**Could a frontier lab, agent vendor, or platform ship your product as a feature within 12 months?**

Name the specific overlapping feature if one exists (e.g., "Claude Code agent view" for a terminal runtime, "GPT-4 with JSON mode" for a structured output API).

- **0**: Easily (pure prompt + API call)
- **1**: Partly (they could ship a basic version)
- **2**: Not without your assets (requires your data, distribution, or integrations)

*Rationale*: If OpenAI could add a button to ChatGPT and replicate your product, you're in trouble. Named after Sherlock Holmes, the doomed AI scheduling startup that Anthropic shipped as an MCP server.

### Question 8: Distribution
**Do you own a channel or customer relationship?**

- **0**: None (open-source tool, API wrapper)
- **1**: Weak (some users, early community, OSS stars)
- **2**: Strong (owned channel, enterprise contracts, viral loop)

**Verify traction** using objective signals:
- GitHub API for stars and forks
- npm, PyPI, or crates.io for package downloads
- Press coverage or investor disclosures

Self-reported traction only (website claims, pitch decks) = cap confidence at **low**.

*Rationale*: Distribution is durable. If 10,000 teams use your product daily, they won't all switch when GPT-5 ships. But if you have 3 GitHub stars and the founder's claim of "strong traction," that's not a hull.

### Question 9: Workflow Embedding
**How deeply are you integrated into user workflows?**

- **0**: Standalone tool (users visit when they remember)
- **1**: Some integrations (Slack bot, VS Code extension)
- **2**: System of record or deep integration (owns the workflow, becomes the source of truth)

*Rationale*: A Slack bot is easier to replace than a system of record. If your product stores the authoritative version of customer data, contracts, or project state, switching costs are high.

### Question 10: Problem Ownership
**What level of the problem do you own?**

- **0**: Task (one API call)
- **1**: Workflow (multi-step process)
- **2**: Outcome (end-to-end responsibility, you own metrics)

*Rationale*: Task products commoditize as models improve. Outcome products capture value as models make the task cheaper. If you own "hiring outcomes" instead of "resume screening," you can swap in better models and keep the margin.

## 5. Pass 2: Fill the Hull Gaps

For low-confidence hull scores, ask the user/founder short questions (one at a time) or research:
- Market and competitors
- Lab roadmaps (Anthropic, OpenAI, Google)
- Recent feature launches that overlap with the product

Update scores and confidence labels.

*Rationale*: Hull is harder to assess from code alone. You need to know whether traction claims are real, whether labs are building competing features, and whether the product compounds value over time.

## 6. Result Calculation

### Normalize
B = H/10 - S/10

Range: -1 (sinking) to +1 (buoyant)

### Quadrant Assignment
- **Buoyant**: H ≥ 6, S ≤ 4
- **Anchored**: H ≥ 6, S ≥ 5
- **Dissolvable**: H ≤ 5, S ≤ 4
- **Sinking**: H ≤ 5, S ≥ 5

**Mark borderline** when within 1 point of a threshold (e.g., H=5 or H=7, S=4 or S=5).

### Gut Check
"Greed or dread when the next model drops?"

- Greed = buoyant (you can't wait to swap in GPT-5)
- Dread = sinking (GPT-5 will break your prompts and make half your code obsolete)

### Trend
If you have earlier scores, compare. Rising B after a release = floating.

*Rationale*: Buoyancy isn't static. A v1 product might be sinking, then v2 deletes scaffolding and becomes buoyant.

## 7. Report Format

Deliver:

1. **One-paragraph verdict**: Quadrant, type, which repo was scored
2. **Score table**: All 10 questions with evidence and confidence labels, plus M
3. **Delete or simplify actions**: 3-5 concrete recommendations to reduce S
4. **Build a hull actions**: 2-3 recommendations to increase H
5. **Caveats**: Stage, short history, unverified traction, borderline flags

**Never modify the repo or post anything publicly without permission.**

*Rationale*: The assessment is diagnostic. You're telling the builder what to fix, not fixing it for them.

## Version History

See [CHANGELOG.md](CHANGELOG.md) for version history and evolution of the framework.
