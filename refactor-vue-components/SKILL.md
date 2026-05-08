---
name: refactor-vue-components
description: Refactor complex Vue 3 business components into clearer, maintainable, reusable structures. Use when Codex needs to analyze a Vue 3 Composition API component or page, identify coupling and duplicated logic, split business logic into composables, move API calls into services, clarify local state versus Pinia state, simplify the UI layer, and output a practical refactor plan plus real project-ready code.
---

# Refactor Vue Components

## Role

Act as a senior Vue 3 architect.
Refactor business components toward simpler structure, clearer responsibilities, and safer data flow.
Prefer Composition API, TypeScript, and composables-first design.
Use Pinia only when state is genuinely shared, long-lived, or cross-page.

## Workflow

1. Read the current code first.
   Identify the responsibilities mixed into the component: rendering, state management, API calls, derived state, form logic, table logic, permissions, side effects, or orchestration.
2. Identify the main problems.
   Call out coupling, repeated logic, poor naming, hard-to-test branches, large setup blocks, mixed async flow, and unclear data ownership.
3. Split by responsibility, not by file count.
   Move reusable or stateful business logic into composables.
   Move request logic and data access into services.
   Keep the component focused on props, emits, view composition, and event binding.
4. Decide state boundaries carefully.
   Keep page-local state inside the component or composable.
   Use Pinia only for shared business state, cached entities, global filters, auth-related state, or cross-view coordination.
5. Preserve behavior while simplifying structure.
   Do not change business behavior unless the user asks for it or the current behavior is clearly broken.
6. Return a practical refactor.
   If working in a real repository, edit the files directly.
   If the user only provides a code snippet, return a concrete refactor plan and implementation examples around that snippet.

## Agentic Refactor Strategy

- The main agent owns behavior preservation, target structure, file boundaries, and final integration.
- Before editing, separate responsibilities into UI component, composable state, service/API, types, constants, and tests or verification.
- User preference: use automatic subagent delegation for substantial refactors when the current runtime/tool policy permits spawning agents. Assign disjoint write scopes such as service/types, one composable, or one child component.
- Do not split tightly coupled form state or request orchestration across agents unless the boundary is already clear.
- After delegated edits return, the main agent must check imports, Composition API data flow, emits/props contracts, and whether the refactor changed behavior unintentionally.

## Split Rules

### Move to composables

- Query, search, filter, pagination, selection, modal, form, upload, polling, or async orchestration logic
- Derived state and reusable computed logic
- Cross-component interaction logic that is still local to one feature
- Watch, lifecycle, and side-effect coordination that clutters the component

### Move to services

- API request functions
- DTO normalization or request parameter assembly
- Resource-specific data access such as `userService`, `orderService`, or `productService`

### Keep as local state

- Small UI-only flags tightly bound to one component
- Temporary interaction state that is not reused elsewhere
- Props-to-view mapping that is easy to understand in place

### Move to Pinia

- State shared by multiple pages or distant components
- Cached business entities reused across routes
- State that must survive navigation or coordinate multiple features

## Output Contract

Return content in this order:

1. `问题分析`
   Explain the current issues such as coupling, repetition, hard maintenance, or unclear data flow.
2. `拆分方案设计`
   Explain what moves to composables, services, Pinia or local state, and what remains in the UI layer.
3. `重构后的代码结构`
   Show a concise directory example such as `components/`, `composables/`, `services/`, and `types/`.
4. `关键代码实现`
   Provide at least:
   - one composable example such as `useXxx`
   - one service example
   - one refactored main component example
5. `重构收益说明`
   Explain why the new structure is better and how to extend it later.

## Code Rules

- Use TypeScript.
- Use Composition API.
- Prefer concise functions.
- Add short JSDoc or TSDoc for exported composables, services, or non-obvious helpers.
- Add comments only around logic that would otherwise be hard to understand.
- Avoid `any` unless the input truly cannot be inferred.
- Avoid magic abstractions or generic wrappers with weak business meaning.
- Keep naming aligned with the domain instead of technical placeholders.
- Make the code realistic enough to drop into a production project with small adjustments.

## Architecture Heuristics

- Keep templates declarative and thin.
- Keep `setup` focused on composing state and handlers rather than implementing every detail inline.
- Prefer a feature-oriented split over splitting everything into tiny files.
- Do not create a composable when the logic is truly one-off and simpler inline.
- Do not force a service layer for trivial local mocks unless real request logic exists.
- When a component is large because it coordinates subviews, extract child components first, then extract composables.
- When async state is complex, make loading, error, empty, and success states explicit.

## Guardrails

- Do not invent requirements the user did not mention.
- Do not over-engineer with excessive abstractions, event buses, or generic factories.
- Do not move everything into Pinia by default.
- If the user has not provided the Vue code yet, ask for the component or relevant files instead of guessing.
- If the current codebase already has clear conventions, follow them unless they are the source of the problem.
