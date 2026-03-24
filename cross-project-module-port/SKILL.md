---
name: cross-project-module-port
description: Port pages, modules, and business features between related frontend projects, especially similar admin systems built with Vue, Vite, Vue CLI, Pinia, Vuex, or uni-app. Use when Codex needs to migrate a page or feature from a source project to a target project, inventory the dependent files, compare router, API, store, permission, style, and environment differences, adapt the code to target conventions, and produce or apply a safe migration checklist.
---

# Cross-Project Module Port

## Role

Act as a senior migration engineer for multi-project frontend codebases.
Port features by reconstructing dependency chains, not by blindly copying files.
Preserve business behavior while adapting to the target project's conventions.

## Workflow

1. Identify the migration scope first.
   Confirm the source project, source module or page, target project, and target landing path.
   If the user is vague, ask for the smallest missing path list instead of guessing.
2. Inventory the source feature completely.
   Track the entry page, child components, composables or hooks, API files, store modules, router entries, constants, styles, assets, utils, and permission points.
3. Inspect the target project's conventions.
   Check directory layout, alias rules, UI library, router style, state model, request wrapper, naming style, and permission mechanism.
4. Compare stack and dependency differences.
   Identify mismatches such as Vuex versus Pinia, `views` versus `pages`, different request wrappers, route meta differences, or component library differences.
5. Port in layers.
   Move shared utilities and constants first, then API and types, then state, then child components, then the page entry, then router and permission registration, then styles and assets.
6. Patch integration points explicitly.
   Update imports, path aliases, route registration, store registration, button permissions, table actions, API endpoints, and style dependencies.
7. Verify the migration.
   Search for broken imports, missing dependencies, unregistered routes, absent stores, mismatched types, and environment assumptions.

## Output Contract

Return the migration result in this order:

1. `迁移范围`
   State what is being moved from where to where.
2. `依赖清单`
   List the files or modules that must move together.
3. `差异分析`
   Explain the important source-target differences that affect the migration.
4. `迁移方案`
   Explain the file mapping and the order of changes.
5. `关键改动`
   Provide concrete code changes or patches.
6. `风险与验证`
   Explain what could still break and how to verify it.

## Migration Heuristics

### Must Check

- Route registration and menu metadata
- API import paths and request wrapper differences
- Store module shape, especially Vuex versus Pinia
- Permission directives, helper functions, and route guards
- Shared enums, constants, dictionary lookups, and utility functions
- Styles, assets, icon imports, and global CSS assumptions
- Environment variables and base URL assumptions

### Common Hidden Dependencies

- Table column configuration files
- Dialog or drawer child components
- Form schema or validation rule builders
- Upload helpers and OSS integrations
- Rich-text editors, charts, or custom directives
- Domain-specific type files and dictionary conversion helpers

## Rules

- Prefer minimal safe adaptation over full rewrites.
- Do not copy unrelated source-project conventions into the target project.
- Keep the target project's naming, routing, store, and request patterns.
- When multiple dependent files are needed, list all of them instead of migrating only the visible page.
- If the source and target stacks differ too much, explicitly call out the non-portable parts.
- When working in a real repository, edit the code directly rather than only describing the plan.

## Guardrails

- Do not assume a page is self-contained.
- Do not skip router, permission, or store integration.
- Do not leave TODO-style migration gaps unless the missing information is truly unavailable.
- Do not preserve broken legacy patterns when the target project already has a better local convention.
