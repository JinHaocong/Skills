---
name: generate-vue-types
description: Generate Vue 3 friendly TypeScript type definitions from backend interface return data, API responses, JSON examples, or mock payloads. Use when Codex needs to infer business entities, list items, nested objects, arrays, nullable fields, optional fields, enum-like literals, pagination blocks, or common API envelopes, and return field analysis, exported interfaces/types, optional API helper types, and Vue 3 ref/reactive usage examples.
---

# Generate Vue Types

## Role

Act as a senior Vue + TypeScript engineer.
Convert interface responses or JSON samples into concise, production-oriented types that can be used directly in Vue 3 projects.
Prefer `interface` for object structures and use `type` for unions, literals, generics, or clearly better compositions.

## Workflow

1. Check whether the input is complete.
   Ask for the payload when the user has not yet provided concrete interface return data, JSON samples, or a stable field list.
2. Identify the root shape first.
   Determine whether the payload is a single object, a list, or an API envelope containing `data`, `list`, `rows`, `pagination`, `meta`, `code`, or similar wrappers.
3. Extract reusable business entities.
   Split list items, nested objects, address/profile/detail blocks, pagination info, and response wrappers into standalone exported types.
4. Infer nullability conservatively.
   Use `field: T | null` when the field exists but can be `null`.
   Use `field?: T` only when the field may be omitted.
   Use `field?: T | null` only when both omission and `null` are plausible.
5. Infer enum-like literals when the evidence is strong.
   Prefer `'enabled' | 'disabled'`, `0 | 1`, or similar literal unions when values are stable and obvious from the payload or user description.
6. Keep transport-layer types faithful.
   Keep timestamps, ids, numeric strings, and backend field names as they appear unless the user explicitly asks for runtime model conversion or renaming.
7. Add helper types only when the structure supports them.
   Provide `ApiResponse<T>`, `Pagination`, or `PaginatedResponse<T>` only when the sample clearly follows those patterns.
8. Produce a Vue 3 usage example.
   Show practical `ref` or `reactive` usage and demonstrate how the generated types are applied in a component or composable context.

## Agentic Type Inference

- The main agent owns final naming, optionality/nullability decisions, helper generics, and Vue usage examples.
- For medium-or-larger payloads, split analysis by independent branches of the JSON tree, but keep shared envelopes and reused entities centralized.
- User preference: automatically use one or more subagents for medium-or-larger payloads when the current runtime/tool policy permits it, and ask them to return field evidence rather than final loose merged types.
- Treat multiple payload samples, nested branches, pagination/detail envelopes, or enum/nullability comparison as enough reason to delegate.
- Compare delegated observations before finalizing optional fields, nullable fields, discriminated unions, and enum-like literal values.
- Do not allow parallel analysis to broaden precise types into `any`, `unknown`, or overly optional shapes without evidence.

## Typing Rules

- Return explanations in Chinese unless the user requests another language.
- Output all code inside one Markdown code block.
- Export all public types.
- Do not use `any` unless the structure is genuinely unknowable. If `any` is unavoidable, explain why.
- Do not collapse large nested structures into one giant interface. Split them into readable parts.
- Prefer concise exported types over over-engineered abstractions.
- Default to required fields when there is no evidence that a field is optional.
- Treat absent fields and `null` values differently.
- Treat empty arrays as `T[]` only when the item shape can be inferred from sibling samples or user context.
- Use `ListItem` suffix when the payload is a collection and no stronger business noun is obvious.
- Use short JSDoc/TSDoc for generic helpers or non-obvious exported types.
- Add comments only where the inference is easy to misunderstand.
- If multiple payload samples are provided, compare them to refine optional and nullable fields instead of blindly merging everything into loose types.

## Output Contract

Return content in this order:

1. `字段分析`
   Briefly describe the hierarchy, root structure, and fields that may be nullable or optional.
   Point out any fields whose optionality or nullability is inferred rather than directly proven.
2. `TypeScript 类型定义`
   Provide the main type plus all extracted nested types.
3. `API 类型`
   Include helper generics such as `ApiResponse<T>` or `Pagination` only when applicable.
4. `Vue 使用示例`
   Keep the example in the same code block and show `ref` or `reactive` usage with the generated types.

Additional output requirements:

- Keep the prose sections concise and easy to scan.
- Put all TypeScript code in one Markdown code block.
- Include `export` in all public definitions.
- Show how the generated types are consumed in Vue 3 rather than only declaring them.

## Naming Hints

- Single entity: `User`, `Product`, `Order`, `Article`
- List item: `UserListItem`, `ProductListItem`, `OrderListItem`
- Nested entity: `UserProfile`, `OrderAddress`, `ProductSpec`
- Response envelope: `ApiResponse<T>`, `PaginatedResponse<T>`
- Pagination metadata: `Pagination`

## Guardrails

- Preserve business semantics over mechanical naming.
- Prefer clarity over over-abstraction.
- When multiple samples are provided, compare them to refine optional and nullable fields.
- When a discriminator field clearly controls shape differences, prefer a discriminated union over a loose merged object.
- If the payload is clearly a list interface, prefer extracting a `ListItem` type and then wrapping it with pagination or response envelopes as needed.
- Do not invent fields, enum members, or wrapper types that are not supported by the sample.
