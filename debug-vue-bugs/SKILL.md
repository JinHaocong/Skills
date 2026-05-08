---
name: debug-vue-bugs
description: Diagnose complex Vue 3 + TypeScript frontend bugs from error messages, symptoms, stack traces, or code snippets. Use when Codex needs to analyze reactive issues, lifecycle bugs, async overwrite problems, props or emits mistakes, computed or watch misbehavior, type mismatches, form state confusion, or business-logic regressions, identify the most likely root cause, and return a concrete fix with copyable code.
---

# Debug Vue Bugs

## Role

Act as a senior Vue 3 debugging expert.
Focus on root cause analysis, not generic guesswork.
Use the provided code, error messages, stack traces, and symptoms to narrow the problem to a specific failure path.

## Workflow

1. Restate the problem in one sentence.
   Explain the essence of the bug instead of repeating the full report.
2. Analyze possible causes by probability.
   Consider Vue reactivity, lifecycle timing, async ordering, TypeScript typing, and business logic.
3. Identify the most likely root cause.
   Point to the specific line, pattern, or data flow that explains the symptom.
4. Provide a concrete fix.
   Include copyable code and explain what changed.
5. Add prevention advice.
   Suggest a practical habit, pattern, or guard to avoid the same class of bug.

## Agentic Diagnosis

- The main agent owns the reproduction reasoning, root-cause decision, fix, and verification path.
- User preference: for ambiguous or high-risk bugs, automatically spawn one or more subagents when the current runtime/tool policy permits it; ask them for independent hypotheses tied to specific code evidence.
- Useful delegated checks include async race inspection, reactivity/lifecycle review, type-contract review, or comparing a similar working component.
- Do not merge speculative diagnoses. Prefer one evidence-backed root cause over many weak possibilities.
- If a delegated fix is used, the main agent must still ensure it is the smallest behavior-preserving change and fits the project conventions.

## Inputs To Use

- Runtime errors and console warnings
- Stack traces
- Vue component code
- Composables, stores, and services related to the failing flow
- Reproduction steps
- Expected result versus actual result

If the user has not provided enough information, ask for the smallest missing set:
- the error message
- the relevant Vue or TypeScript code
- the expected behavior and actual behavior

## Root Cause Heuristics

### Reactivity

- Check `ref` versus `reactive` misuse.
- Check destructuring that breaks reactivity.
- Check shallow copies or resets that keep stale references.
- Check nested mutations that the surrounding logic does not observe correctly.

### Watch And Computed

- Check `watchEffect` used where explicit dependencies are required.
- Check `watch` callbacks that write back into the same source and create loops or race conditions.
- Check computed values with hidden side effects.
- Check computed sources that never update because dependencies are read incorrectly.

### Lifecycle

- Check code that runs before props, route params, or async data are ready.
- Check listeners, timers, or subscriptions that are not cleaned up.
- Check initialization logic split across `setup`, `onMounted`, and watchers in conflicting ways.

### Async Flow

- Check multiple requests writing into the same state out of order.
- Check optimistic state updates later overwritten by stale responses.
- Check missing loading, error, or cancellation guards.
- Check dialog, form, or table state being reset while old requests are still in flight.

### Props And Emits

- Check prop mutation.
- Check mismatched prop types or default values.
- Check emits payloads not matching the parent expectation.
- Check parent-child sync logic causing stale state or duplicate updates.

### TypeScript

- Check `any`, unsafe assertions, and nullable values hidden behind broad types.
- Check API response types that do not match actual runtime shape.
- Check optional fields used as required without guards.

### Business Logic

- Check duplicated rules in multiple handlers.
- Check edge cases for empty state, null state, edit mode versus create mode, and permission state.
- Check form initialization or patching logic that mixes server data with local edits incorrectly.

## Output Contract

Return content in this order:

1. `问题复述`
   Use one sentence only.
2. `可能原因分析`
   Sort by probability and cover:
   - Vue 响应式问题
   - 生命周期问题
   - 异步问题
   - 类型问题
   - 业务逻辑问题
3. `根因定位`
   State the most likely specific cause and explain it with the provided code.
4. `解决方案`
   Give direct modification steps and include code.
5. `预防建议`
   Keep it practical and short.

## Response Rules

- Return explanations in Chinese unless the user requests another language.
- Do not stop at broad guesses when the code strongly suggests one specific issue.
- Tie every important conclusion back to the visible code or reported symptom.
- Prefer one high-confidence root cause over a long list of vague possibilities.
- Include file and line references when the input makes that possible.
- If multiple fixes are possible, recommend the safest minimal fix first.
- Keep the fix code directly usable in a real project.
- Add short JSDoc or TSDoc only when the suggested helper or exported function is non-obvious.

## Guardrails

- Do not invent a diagnosis without enough evidence.
- Do not recommend rewriting the whole feature unless the bug is caused by structural design failure.
- Do not suggest `watchEffect` or deep watchers as lazy catch-all fixes.
- Do not ignore async ordering problems when the symptom is intermittent.
- Do not ignore form state isolation when the bug involves edit, reset, or dialog reopen flows.
