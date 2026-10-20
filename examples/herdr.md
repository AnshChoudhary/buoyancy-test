# Herdr Buoyancy Assessment

**Framework version**: v0.4

**Target**: [github.com/herdrdev/herdr](https://github.com/herdrdev/herdr) (YC F26)  
**Type**: Infra  
**Stage**: Early product  
**Date**: October 2026

## Verdict

**Buoyant** (S=3/10, M=1/2, H=7/10, B=+0.4)

Herdr is a terminal runtime for coding agents. Minimal scaffolding—no model calls in the runtime itself, just infrastructure for connecting agents to terminals. Strong hull through workflow embedding (agents run inside it) and distribution (YC batch, early enterprise interest). Some manifests brittleness in screen-text detection. Overlaps with Claude Desktop Code agent view (Sherlock risk).

---

## Scaffolding (S) = 3/10

### 1. Delete test: 0/2
**<10% deletable**

No model calls in the core runtime. Herdr is plumbing: it exposes terminal sessions to agents. Better models don't obsolete terminal emulation, session management, or security sandboxing.

**Evidence**: Infrastructure layer, not model orchestration.

### 2. Prompt sediment: 0/2
**Few (not applicable)**

No prompts in the runtime itself. Agents bring their own prompts.

**Evidence**: Runtime doesn't generate prompts.

### 3. Dependent chain depth: 0/2
**1-2 hops (not applicable)**

No model chains. Agents call the runtime, runtime manages terminals.

**Evidence**: Synchronous API calls, no model orchestration.

### 4. Swap cost: 0/2
**Config only (not applicable)**

No model dependencies in the runtime. Agents handle their own model switching.

**Evidence**: Model-agnostic infrastructure.

### 5. Dependency brittleness: 1/2
**Some, with tests**

Screen-text detection for terminal state parsing is inherently brittle. If terminal emulators change output format (ANSI codes, cursor positioning), herdr may break. Has tests and fallbacks, but this is a core risk for terminal runtimes.

**Evidence**: Terminal parsing logic relies on ANSI escape codes and shell output formats.

---

## Measurability (M) = 1/2

**Tests or spot checks**

Likely has integration tests (spinning up terminals, executing commands, checking outputs). Not model-version evals (since no models in the runtime), but can prove functionality works.

**Evidence**: Standard infra testing (estimated).

---

## Hull (H) = 7/10

### 6. Proprietary/compounding context: 1/2 (confidence: medium)
**Some proprietary context**

Per-user terminal sessions, command history, environment state. Could evolve to store agent interaction history, learned user preferences for terminal configs.

**Evidence**: Session management implies stored state.

### 7. Sherlock risk: 1/2 (confidence: medium)
**Partly**

Claude Desktop ships a Code agent view with terminal access. GitHub Copilot Workspace has terminal integration. Labs could add terminal primitives to agent APIs within 12 months.

However, Herdr's enterprise security sandboxing, session management, and multi-user orchestration are harder to replicate.

**Evidence**: Overlapping feature exists (Claude Code agent terminal), but Herdr's infra depth is defensible.

### 8. Distribution: 2/2 (confidence: high)
**Strong**

YC F26 batch. **44,900 GitHub stars** (verified via GitHub API). Early enterprise customers (verifiable via YC directory and press). Terminal runtimes have network effects—agents integrate with herdr, creating switching costs.

**Evidence**: 44.9k GitHub stars (verified), YC affiliation, early traction signals.

### 9. Workflow embedding: 2/2 (confidence: high)
**Deep integration**

Agents run *inside* Herdr. It's the execution environment. Switching costs are high—agents would need to rewrite terminal interaction code.

**Evidence**: Runtime layer = system of record for agent execution.

### 10. Problem ownership: 1/2 (confidence: medium)
**Workflow**

Owns the "agent terminal access" workflow, not the outcome (code correctness, deployment success). Agents own the outcome; Herdr is the pipe.

**Evidence**: Infrastructure, not end-to-end solution.

---

## Normalized Score

**B = H/10 - S/10 = 7/10 - 3/10 = +0.4**

**Quadrant**: Buoyant (H ≥ 6, S ≤ 4)

**Gut check**: Greed. Better models (GPT-5, Claude 4) will make agents smarter, and Herdr captures that value by being the runtime they run on. No dread about model upgrades breaking things.

---

## Recommendations

### Delete or simplify (reduce S):
1. **Invest in terminal parsing robustness**: If screen-text detection is brittle, add more comprehensive tests or move to structured terminal APIs where possible.
2. **Document terminal compatibility matrix**: Which emulators, shells, and output formats are supported? Reduces dependency brittleness by making assumptions explicit.

### Build a hull (increase H):
1. **Agent interaction history**: Store which agents succeeded at which tasks, learn per-user preferences (raises Q6 to 2/2).
2. **Outcome ownership**: Offer "verified deploys" or "code correctness guarantees" on top of terminal access (raises Q10 to 2/2).
3. **Platform network effects**: Build an agent marketplace or directory where agents advertise Herdr compatibility, locking in distribution (raises Q8 defensibility).

---

## Caveats

- **Stage**: Early product, so some hull scores (Q6, Q10) could rise as the product matures
- **Sherlock risk**: Claude Desktop Code agent view is a direct competitor; monitor lab roadmaps closely
- **Distribution moat**: 44.9k stars create defensibility even with Sherlock overlap
