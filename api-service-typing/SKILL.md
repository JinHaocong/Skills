---
name: api-service-typing
description: Create typed API and service layers for frontend projects from interface docs, JSON samples, or existing request code. Use when Codex needs to define request params, response envelopes, pagination models, DTO types, service adapters, model transformations, and error-handling conventions, and should produce a clean transport layer plus a business-friendly service layer instead of only raw TypeScript interfaces.
---

# API Service Typing

## Role

Act as a frontend architect for typed request and service design.
Separate transport-layer DTOs from business-facing models when that split creates clarity.
Generate code that matches the current repo's request wrapper and file layout.

## Workflow

1. Inspect the repo conventions first.
   Check the existing request wrapper, API folder layout, error handling style, and naming conventions.
2. Identify the envelope pattern.
   Determine whether the backend uses wrappers such as `data`, `rows`, `list`, `records`, `code`, `msg`, or `success`.
3. Define the type layers.
   Split request params, raw response DTOs, list items, pagination models, and business-facing models as needed.
4. Generate API functions.
   Keep the API layer close to the backend contract and request details.
5. Generate service adapters.
   Map DTOs into cleaner business models only when the transformation is meaningful.
6. Normalize shared patterns.
   Reuse helpers for pagination, common envelopes, and consistent error handling.
7. Show usage clearly.
   Demonstrate how components, composables, or stores consume the typed API and service layer.

## Output Contract

Return or implement these parts when applicable:

1. `类型设计`
   Explain the key DTO, request, response, and pagination types.
2. `API 层`
   Include typed request functions.
3. `Service 层`
   Include adapters or business-facing helpers.
4. `错误处理约定`
   Explain how failures should be surfaced consistently.
5. `使用示例`
   Show how the generated code is consumed.

## Design Rules

- Prefer `interface` for object types and `type` for unions or generic helpers.
- Do not use `any` unless the structure is truly unknowable.
- Keep backend DTO field names faithful in the API layer.
- Put field renaming, normalization, or unit conversion in the service layer.
- Reuse shared `ApiResponse<T>`, `Pagination`, or `PaginatedResult<T>` helpers when the backend contract supports them.
- Distinguish optional fields from nullable fields.
- Keep request params and response DTOs separate when their shapes differ.

## Common Patterns

- List APIs with pagination and filters
- Detail APIs with nullable or partial fields
- Create, update, enable, disable, and delete flows
- Upload or import APIs with custom result envelopes
- Dictionary or enum APIs that need UI-friendly mapping
- Error-code-based business failures that should be normalized for UI consumption

## Guardrails

- Do not collapse DTOs, request params, and UI models into one loose type.
- Do not hide transport details inside the service layer without keeping the API contract visible.
- Do not invent pagination or envelope helpers if the backend does not actually use them.
- Do not over-map fields when the raw DTO is already suitable for the UI.
