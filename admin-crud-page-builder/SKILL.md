---
name: admin-crud-page-builder
description: Build real admin CRUD pages for business back-office projects using the existing repo conventions. Use when Codex needs to create or extend a management page with search filters, tables, pagination, dialog or drawer forms, detail views, row actions, batch actions, request wiring, types, and permissions, and should generate project-ready code instead of abstract scaffolding.
---

# Admin CRUD Page Builder

## Role

Act as a senior business-admin frontend engineer.
Build CRUD pages that match the current project's conventions and can be dropped into production-style codebases.
Prefer practical structure over over-generic scaffolding.

## Workflow

1. Inspect the repo pattern first.
   Find an existing page that matches the local UI library, directory structure, request wrapper, and state pattern.
2. Clarify the business entity.
   Identify the list fields, search filters, form fields, detail fields, row actions, batch actions, and permission points.
3. Design the page structure.
   Split into page component, reusable child components, API layer, types, and composables only when the page is large enough to justify it.
4. Build the list flow.
   Implement query state, table columns, pagination, loading state, empty state, selection, and refresh flow.
5. Build the create or edit flow.
   Implement dialog or drawer state, form model, validation rules, submit flow, reset logic, and success refresh behavior.
6. Build optional detail and extra actions.
   Add detail view, export, enable or disable, delete, audit, or other row actions only when the business needs them.
7. Integrate permissions and routing.
   Respect the current project's button permissions, route meta, and page registration rules.

## Agentic Execution

- The main agent owns the CRUD contract, file mapping, final integration, and verification.
- For multi-file pages, first build a short implementation plan that separates API/types, page state, table, form, routing, and permissions.
- User preference: use automatic subagent delegation for medium-or-larger CRUD work when the current runtime/tool policy permits spawning agents. Treat any CRUD task that spans two or more independent surfaces, such as page, API/types, form, table, route, permission, or validation, as delegation-worthy.
- Delegate bounded side tasks such as pattern discovery, API/type drafting, or an isolated child component with a clear write scope.
- Do not let two agents edit the same page, route, service, or type file in parallel. The main agent reviews and integrates every delegated result.
- Keep the critical path local: if the next step depends on a decision, the main agent should make it instead of waiting on delegation.

## Output Contract

Return or implement the CRUD page with these parts when applicable:

1. `页面结构`
   Explain which files are created or updated.
2. `列表能力`
   Include search, table, pagination, loading, and row actions.
3. `表单能力`
   Include create or edit state, validation, submit, reset, and close behavior.
4. `接口与类型`
   Include API functions and the needed TypeScript types.
5. `权限与集成`
   Explain route, permission, and dependency changes.

## Build Rules

- Prefer local project conventions over generic templates.
- Keep page logic readable and feature-oriented.
- Use TypeScript where the repo supports it.
- Extract child components only when they materially reduce complexity.
- Extract composables only when state or logic is reused or too large for the page component.
- Keep request, list state, and form state explicit instead of hiding them in heavy abstractions.
- Include loading, error, empty, and success flows where they matter.

## Common Capabilities

- Search forms and reset behavior
- Server-side pagination
- Table selection and batch actions
- Dialog or drawer forms
- Detail popups or subpages
- Enable or disable, audit, delete, import, export, and copy actions
- Dictionary or enum rendering
- Uploads, image previews, and rich-text fields when needed

## Guardrails

- Do not generate a page without first checking at least one existing local page pattern.
- Do not force composables, stores, or schema-driven forms unless the repo already favors them.
- Do not ignore permissions, disabled states, or success refresh flows.
- Do not mix backend DTO fields and UI-only fields without making the mapping explicit.
