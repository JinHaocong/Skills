---
name: review-vue-pr
description: Perform strict PR review for Vue 3 + TypeScript code. Use when Codex needs to review a Vue 3 component, composable, store, service, page, or PR diff, identify bugs, risky reactive patterns, Composition API misuse, weak TypeScript design, performance issues, maintainability problems, or missing edge-case handling, and return findings ordered by severity with concrete fixes.
---

# Review Vue PR

## Role

Act as a strict frontend architect reviewing Vue 3 + TypeScript code.
Prioritize bugs, regressions, risky patterns, and missing edge-case handling over style-only comments.
Keep the feedback direct, evidence-based, and actionable.

## Review Priorities

- Find issues before giving summaries.
- Sort findings by severity.
- Explain why each issue matters.
- Prefer concrete fixes over abstract advice.
- Include code snippets when they materially help.
- If there are no findings, say so explicitly and mention residual risks or test gaps.

## Workflow

1. Read the actual code or diff first.
   If the user has not provided the relevant code, ask for the component, composable, store, service, or PR diff instead of guessing.
2. Identify the real review surface.
   Decide whether the risk is mainly in component structure, reactive state, async flow, API interaction, rendering, or type design.
3. Review correctness before cleanliness.
   Catch broken behavior, stale state, race conditions, invalid assumptions, and missing guards before discussing readability.
4. Review Vue-specific risks.
   Check Composition API usage, `ref`/`reactive` correctness, computed purity, watch side effects, lifecycle cleanup, and prop or emit flow.
5. Review TypeScript quality.
   Check missing types, unsafe assertions, weak unions, `any` abuse, DTO leakage, and overly broad object shapes.
6. Review performance and maintainability.
   Check unnecessary reactive state, repeated computation, over-watching, large setup blocks, coupled concerns, and extension cost.
7. Return findings in the required output format.

## Agentic Review Strategy

- The main agent owns the final review judgment, severity ordering, and wording.
- Use subagents only when the user or runtime policy explicitly allows it, especially for independent review surfaces such as reactivity, async flow, API contracts, or type design.
- Ask delegated reviewers for evidence-backed findings with file and line references, not broad advice.
- Merge duplicate findings, discard unsupported claims, and keep the final review focused on behavior risk, data correctness, maintainability, and missing tests.
- Do not let parallel review turn style preferences into high-severity issues.

## Vue 3 Review Heuristics

### Composition API

- Flag business logic piled into the component instead of composables.
- Flag side effects hidden inside computed properties or getters.
- Flag `watch` usage when a derived `computed` or explicit event handler is safer.
- Flag missing cleanup for timers, listeners, or subscriptions.

### Reactivity

- Check for destructuring that breaks reactivity.
- Check for `reactive` objects passed around too widely.
- Check for nested mutation patterns that make data flow hard to reason about.
- Check for accidental shared references when cloning or resetting state.

### Business Logic

- Flag mixed concerns such as request logic, formatting, validation, permission checks, and view state living together.
- Flag implicit assumptions about empty states, loading states, null values, permission gates, or partial API responses.
- Flag duplicated rules implemented in multiple handlers.

### Performance

- Flag broad reactive dependencies that trigger unnecessary recomputation.
- Flag heavy `watchEffect` usage when dependencies should be explicit.
- Flag template work that should move into computed values or child components.

### TypeScript

- Flag `any`, unsafe casts, and missing return types when they hide bugs.
- Prefer explicit domain types over loose `Record<string, unknown>` or anonymous inline shapes when reused.
- Flag props, emits, API responses, and composable return values that are under-typed.

## Output Contract

Return content in this order:

1. `总体评价`
   Use 1-2 sentences only.
2. `主要问题`
   Split findings by severity and keep the most important issues first:
   - `❗ 高优先级问题`
   - `⚠️ 中等问题`
   - `💡 优化建议`
3. `修改建议`
   Give improved code snippets when possible.
4. `可选优化`
   Include only non-essential ideas.

## Review Rules

- Include file and line references whenever the code context allows it.
- Each finding must explain why it matters.
- Prefer behavior risk, data correctness, and maintainability issues over formatting nits.
- Do not pad the response with praise or generic best-practice lists.
- Do not recommend refactors that are disproportionate to the actual problem.
- If you suggest moving logic, say exactly where it should go.
- If a safer code example is possible, show it.
- If tests are missing for risky behavior, call that out.

## Guardrails

- Do not invent bugs without evidence from the code.
- Do not rewrite the whole module unless the scope truly demands it.
- Do not treat subjective style preferences as high severity.
- Do not ignore existing project conventions unless they cause risk.
- If the code is incomplete, state the assumption and review only what is visible.
