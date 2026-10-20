# AI GitHub Profiler Buoyancy Assessment

**Framework version**: v0.4 (earlier assessment, note that hull scores used partly unverified traction claims)

**Target**: [github.com/AnshChoudhary/ai-github-profiler](https://github.com/AnshChoudhary/ai-github-profiler)  
**Type**: App  
**Stage**: Early product  
**Date**: [Assessment date with earlier framework version]

## Verdict

**Dissolvable** (S=2/10, M=0/2, H=2/10, B=0.0)

AI GitHub Profiler is a clean, minimal app that analyzes GitHub profiles using Gemini. Low scaffolding with parallel API calls and native structured output. No defensive prompts or complex chains. However, zero evals and minimal defensibility—public data only, no user context, standalone tool with no integration depth. Easy to replicate and no moat.

---

## Scaffolding (S) = 2/10

### 1. Delete test: 0/2
**<10% deletable**

One 275-line Gemini service file. Uses native structured output (Gemini's response schema), no JSON repair, no re-ask loops, no reviewer models. Three parallel independent calls (commits, PRs, user). Clean orchestration.

**Evidence**: Single model call site, parallel data fetching, structured schema.

### 2. Prompt sediment: 0/2
**Few defensive instructions**

Minimal prompts. No "never/always/must/IMPORTANT" spam. Straightforward system prompts.

**Evidence**: No accumulation of defensive rules.

### 3. Dependent chain depth: 0/2
**1-2 hops**

Three parallel calls = counts as one hop. No sequential dependencies.

**Evidence**: GitHub data → Gemini → response (single logical hop).

### 4. Swap cost: 1/2
**Same-provider upgrade: 0 (config), cross-provider: 2 (migration)**

Model name hardcoded: `gemini-1.5-flash`. Changing to `gemini-1.5-pro` = config. Switching to OpenAI = would need to adapt structured output format (Gemini's response schema differs from OpenAI's JSON mode).

**Score**: max(0, 2/2 rounded down) = max(0, 1) = 1

**Evidence**: Single model reference, but schema format is Gemini-specific.

### 5. Dependency brittleness: 1/2
**Some, with tests**

Relies on GitHub API structure. Uses documented GitHub API endpoints. However, hardcoded assumption `externalPRsMerged = 0` (not fetched from API) shows some brittleness.

**Evidence**: GitHub REST API dependency, hardcoded PR count assumption.

---

## Measurability (M) = 0/2

**No evals**

No tests comparing model versions. No spot checks. Would need to manually test if upgrading Gemini versions improves results.

**Evidence**: No test directory, no eval framework.

---

## Hull (H) = 2/10

### 6. Proprietary/compounding context: 0/2 (confidence: high)
**None**

Stateless. All public GitHub data. No user history, no learned preferences. Every request is independent.

**Evidence**: No database, no session storage, no user accounts.

### 7. Sherlock risk: 0/2 (confidence: high)
**Easily replicable**

GitHub + Gemini API call. Could be a ChatGPT plugin or Claude MCP server in ~1 hour. No unique assets.

**Evidence**: Straightforward API composition, no proprietary algorithms.

### 8. Distribution: 1/2 (confidence: low - unverified)
**Weak**

Early GitHub repo, no verified download counts, no press. Self-reported traction only.

**Evidence**: GitHub stars not verified via API for this assessment.

### 9. Workflow embedding: 0/2 (confidence: high)
**Standalone**

User visits the site, enters a GitHub username, gets a report. No integrations with recruiting tools, LinkedIn, or ATS systems.

**Evidence**: Single-page app with no external integrations.

### 10. Problem ownership: 1/2 (confidence: high)
**Workflow**

Owns the "profile analysis" workflow (fetching + summarizing), not just one API call. But doesn't own hiring or recruiting outcomes.

**Evidence**: Multi-step data fetching and aggregation, but stops at report generation.

---

## Normalized Score

**B = H/10 - S/10 = 2/10 - 2/10 = 0.0**

**Quadrant**: Dissolvable (H ≤ 5, S ≤ 4)

**Gut check**: Neither greed nor dread. GPT-5 won't break anything, but it also won't help much since the code is already clean.

---

## Recommendations

### Delete or simplify (reduce S):
1. **Remove hardcoded model name**: Move `gemini-1.5-flash` to environment variable to reduce swap cost to 0.
2. **Fix hardcoded externalPRsMerged=0**: Fetch actual external PR data from GitHub API or remove the field.
3. **Add small eval**: 10 test profiles with expected output categories to enable confident model upgrades (would raise M to 1).

### Build a hull (increase H):
1. **Store analysis history**: Let users track profile evolution over time (Q6: context → 1-2/2).
2. **LinkedIn + GitHub integration**: Combine public LinkedIn data for richer profiles.
3. **ATS integration or badge**: Let users embed a live profile badge on their resume/portfolio, or integrate with applicant tracking systems (Q9: workflow embedding → 1-2/2).
4. **Deeper signals**: Parse code quality, collaboration patterns, impact (not just counts) to make analysis harder to replicate (Q7: Sherlock risk → 1/2).

---

## Caveats

- **Stage**: Early product, so low hull scores are expected
- **Hull scores**: Based partly on unverified traction claims (low confidence on Q8)
- **Framework version**: Assessed with v0.4 rubric applied retroactively
