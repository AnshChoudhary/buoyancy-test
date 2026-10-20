# YC F26 Buoyancy Study

**Framework version**: v0.4  
**Date**: October 2026  
**Sample**: 16 YC Fall 2026 startups

## Method

This study assesses 16 YC F26 startups using the Buoyancy Test v0.4 framework. Two assessment modes:

### Repo Mode (8 startups)
Full code access via public GitHub repositories. Assessments include:
- Code read: prompt files, model-call sites, chain depth, config, tests/evals
- Git log analysis: commit history, technical debt accumulation
- Integration analysis: vendor dependencies, API usage
- **Higher confidence** on scaffolding (S) and measurability (M) scores

### Buyer Mode (8 startups)
Public information only (websites, docs, demos, press). Assessments include:
- Documentation and changelog review
- Product demos and UI analysis
- Model disclosures and feature descriptions
- **Lower confidence**: scaffolding (S) estimated from observable behavior, measurability (M) typically scored 0, hull (H) scores based on public traction signals

### Traction Verification
Where possible, traction claims verified via:
- GitHub API: stars, forks, commit activity (e.g., herdr: 44.9k stars verified)
- npm/PyPI/crates.io: package downloads
- Press coverage and YC directory

## Results

| Name | Type | Mode | S | M | H | B | Quadrant | Borderline | Confidence |
|------|------|------|---|---|---|-------|----------|------------|------------|
| herdr | Infra | repo | 3 | 1 | 7 | +0.4 | Buoyant | no | high |
| Pomerado | App | repo | 8 | 1 | 5 | -0.3 | Sinking | yes | med |
| Caveman | Router/Optimizer | repo | 5 | 2 | 6 | +0.1 | Anchored | yes | med |
| OpenSEO | App | repo | 3 | 1 | 6 | +0.3 | Buoyant | yes | med |
| Agent Relay | Infra | repo | 3 | 2 | 5 | +0.2 | Dissolvable | yes | med |
| Databuddy | App | repo | 2 | 2 | 7 | +0.5 | Buoyant | no | med |
| OpenHack | App | repo | 4 | 1 | 4 | 0.0 | Dissolvable | yes | med |
| Stacktrace | Infra | repo | 1 | 1 | 4 | +0.3 | Dissolvable | no | low |
| Frontrunner | App | buyer | 3 | 0 | 6 | +0.3 | Buoyant | yes | low |
| AdsideAI | App | buyer | 3 | 0 | 4 | +0.1 | Dissolvable | no | low |
| Roma | App | buyer | 3 | 0 | 4 | +0.1 | Dissolvable | no | low |
| Kairos Health | App | buyer | 3 | 0 | 8 | +0.5 | Buoyant | no | low |
| Lark | App | buyer | 3 | 0 | 7 | +0.4 | Buoyant | no | low |
| Praxis | App | buyer | 3 | 0 | 5 | +0.2 | Dissolvable | yes | low |
| Latent | Router/Optimizer | buyer | 6 | 0 | 4 | -0.2 | Sinking | no | low |
| Papaya | Router/Optimizer | buyer | 3 | 0 | 6 | +0.3 | Buoyant | yes | low |

**Legend**: S = Scaffolding (0-10, lower better), M = Measurability (0-2), H = Hull (0-10, higher better), B = Buoyancy score (H/10 - S/10)

## Quadrant Distribution

![Buoyancy quadrant chart for 16 YC F26 startups](../assets/buoyancy_quadrant_16.png)

- **Buoyant** (H ≥ 6, S ≤ 4): **7 startups** (44%)
  - herdr, OpenSEO, Databuddy, Frontrunner, Kairos Health, Lark, Papaya
- **Dissolvable** (H ≤ 5, S ≤ 4): **6 startups** (37%)
  - Agent Relay, OpenHack, Stacktrace, AdsideAI, Roma, Praxis
- **Sinking** (H ≤ 5, S ≥ 5): **2 startups** (13%)
  - Pomerado, Latent
- **Anchored** (H ≥ 6, S ≥ 5): **1 startup** (6%)
  - Caveman

**Borderline cases** (within 1 point of quadrant threshold): 10 of 16 startups (63%), indicating that small product changes could shift quadrants.

## Key Findings

### 1. Vertical Apps Embedded in Systems of Record Have the Strongest Hulls

**Kairos Health** (H=8), **Lark** (H=7), and **Databuddy** (H=7) achieved the highest hull scores by:
- Becoming the system of record for their domain (healthcare workflows, compliance docs, data analysis)
- Deep workflow embedding: users can't easily replace without losing context and history
- Proprietary compounding context: user data, learned preferences, domain-specific integrations

These products own outcomes, not just tasks. When models improve, they capture the value by delivering better outcomes at the same price point (or maintaining outcomes at lower cost).

**Contrast with task-level products**: AdsideAI, Roma, Stacktrace (H=4) perform isolated tasks without compounding context. Easy to replicate, low switching costs.

### 2. Coding-Agent Infrastructure is Mostly Dissolvable Due to Claude Code Overlap

**Agent Relay** (Dissolvable, B=+0.2), **OpenHack** (Dissolvable, B=0.0), and **Stacktrace** (Dissolvable, B=+0.3) face Sherlock risk from Claude Desktop's Code agent view and GitHub Copilot Workspace:
- Terminal runtimes: Claude Code ships terminal access
- Security review: labs could add sandboxing primitives
- Agent orchestration: commoditizing rapidly

**herdr escapes via verified traction**: 44.9k GitHub stars (verified via API), YC F26 batch, enterprise customers → strong distribution (Q8: 2/2) and workflow embedding (Q9: 2/2) push H to 7. Distribution is a moat even when the product is replicable.

**Lesson**: In fast-commoditizing categories, distribution and early market capture matter more than technical differentiation.

### 3. Most Common Scaffolding: Reviewer/Verifier Models and Swarms

**Pomerado** (S=8), **OpenHack** (S=4), **AdsideAI** (S=3 estimated):
- Multi-agent chains: minter → guardian reviewer → retry loops
- Verifier models that check output quality
- Error recovery and re-ask layers
- Heavy prompt sediment (hundreds of defensive instructions)

These patterns compensate for current model error rates. As models improve (lower error rates, better reasoning), these layers become obsolete. High delete-test scores (Q1: much code becomes deletable with a 2x better model).

**Exception**: Evals and routing are plumbing. **Caveman** (M=2) and **Agent Relay** (M=2) have strong eval frameworks that *appreciate* as models improve—you can confidently delete scaffolding because evals prove the new model is better.

### 4. Dependency Brittleness is the Real Fragility

The highest scaffolding risk isn't prompt complexity—it's **undocumented vendor dependencies** (Q5):

**herdr** (S=3 overall, but Q5 estimated 1-2/2):
- Screen-text detection for terminal state parsing
- If terminal emulators change ANSI codes or cursor positioning, herdr breaks
- Inherent brittleness in terminal runtimes

**Pomerado** (S=8, Q5 high):
- Browser automation for driving third-party sites
- Reverse-engineered site structures
- Breaks when sites redesign or add anti-bot measures

**Lesson**: Scaffolding from prompt tuning is fixable (rewrite prompts). Dependency brittleness creates existential risk (vendor changes something, you break, users churn before you patch).

### 5. Products Built to Fix Model Weaknesses Are at Risk

**Latent** (Sinking, B=-0.2, S=6 estimated):
- Router/optimizer focused on model weakness compensation
- If the product's value prop is "we make bad models usable," better models directly threaten the business
- Contrast with **Papaya** (Buoyant, B=+0.3) and **Caveman** (Anchored, B=+0.1): routers that optimize for cost/speed appreciate as new models expand the efficient frontier

**Gut check question**: When GPT-5 drops, do you feel greed (can't wait to integrate it) or dread (it might obsolete our product)?
- Latent: likely dread (if models are better, why do customers need us?)
- Papaya/Caveman: greed (more models = better routing decisions)

## Implications for Builders

1. **Invest in hull first**: Distribution and workflow embedding are more durable than technical moats. herdr's 44.9k stars create a moat even as Claude Desktop ships overlapping features.

2. **Separate plumbing from scaffolding**: Use evals (M=2) to identify which code appreciates vs depreciates. Delete aggressively.

3. **Avoid dependency brittleness**: Undocumented vendor dependencies (screen scraping, private APIs) are higher risk than prompt complexity.

4. **Own outcomes, not tasks**: Kairos Health (H=8) owns healthcare workflow outcomes. AdsideAI (H=4) performs ad-copy tasks. The outcome product has 2x the hull.

5. **Watch for Sherlock risk**: If your product is "ChatGPT + one API call," you're vulnerable. Build defensibility before labs notice the gap.

## Caveats

- **Scores are approximate**: Intended as framework demos and relative comparisons, not absolute judgments or investment advice.
- **Buyer-mode uncertainty**: 8 startups assessed from public info only. Scaffolding (S) is estimated; actual code may differ.
- **Early stage**: All startups are YC F26 (founded 2026). Hull scores will evolve as products mature.
- **Borderline prevalence**: 10 of 16 are borderline (within 1 point of quadrant threshold). Small product changes could shift categories.
- **Traction timing**: Some traction metrics (GitHub stars, downloads) reflect early momentum, not long-term defensibility.
- **Framework evolution**: v0.5 may refine borderline rules, buyer-mode scoring, and evidence requirements.

---

**Methodology note**: This study demonstrates the Buoyancy Test framework applied to a cohort. Individual scores are illustrative. For investment or vendor selection decisions, conduct a full assessment with code access and founder interviews.
