---
name: keycloak-mui-theme
description: Create, modify, or review Keycloak themes implemented from official Keycloak source contracts with Material UI and latest stable Storybook coverage. Use when Codex works on Keycloak/Keycloakify theme code, especially login, email, admin, account, or other explicitly requested theme scopes; FTL parity for FTL-based themes; React console parity for admin/account themes; MUI component usage; button and typography consistency; MUI-native colors and icons; Storybook setup using the latest stable release channel; stories matching official Keycloak original pages, routes, shell, state models, and interactions; dependency choices for current MUI controls; or GitHub Actions updates that avoid Node20 deprecation warnings by using current action majors and Node24-compatible runtimes.
---

# Keycloak MUI Theme

## Overview

Use this skill to keep Keycloak theme work focused, MUI-native, and maintainable. Default to login theme only unless the user explicitly asks for another Keycloak theme type.

## Scope Rules

- Treat `login` as the only supported theme scope by default for newly created theme scopes.
- Never delete, move, disable, overwrite, or de-register existing Keycloak theme scopes, configuration, files, Storybook stories, build entries, or package scripts as a way to satisfy the default login-only rule.
- Preserve existing `account`, `admin`, `email`, or other non-login theme scopes unless the user explicitly asks to remove that scope.
- If a user asks to inspect, review, or fix the whole project, treat existing configured theme scopes as intentionally supported. Fix them in place according to this skill instead of removing them.
- Do not create new `account`, `admin`, `email`, or other Keycloak theme types unless the user names that scope or approves expanding beyond login.
- When the requested scope is ambiguous and no non-login scope already exists, implement the login theme path only and note the assumption.
- When the requested scope is ambiguous but non-login themes already exist, preserve them and either fix them if the request covers the whole project or report them as out of scope. Ask before making scope-removal changes.
- Keep non-login theme code isolated from login theme code when an expanded scope is requested.

## Required Official Source Workflow

- Resolve the target Keycloak version from the repository first, such as Keycloak dependencies, Keycloakify configuration, Docker images, documentation, or CI. If no version is discoverable, use the latest stable official Keycloak source only after noting that assumption.
- Choose the correct official source contract by theme scope:
  - Use official FTL files for FTL-based scopes such as `login` and `email`.
  - Use the official React console source, routes, shell, page modules, state model, and Admin REST interaction patterns for React console-based scopes such as `admin` and `account`.
- Do not invent page or route coverage from memory. Do not omit official templates, routes, or console modules because they are uncommon unless the user explicitly excludes them.
- Preserve existing custom pages or modules that extend official behavior. Bring them into parity with the appropriate official contract and add stories instead of deleting or replacing them wholesale.

## FTL-Based Theme Workflow

- Before creating a new `login`, `email`, or other FTL-based theme/page implementation, read the official Keycloak FTL files for the target Keycloak version and theme scope.
- Use the official FTL files as the implementation contract for page coverage, required variables, form actions, message keys, conditional branches, IDs/classes with behavioral meaning, links, scripts, and accessibility affordances.
- If the user explicitly names one or more FTL files, implement those files and any shared layout/macros needed by them.
- If the user asks to create an FTL-based theme but does not explicitly name a concrete FTL file or subset, implement every official FTL file for the supported FTL-based scope.
- For every implemented official FTL file, add corresponding Storybook stories that cover the official default state and meaningful official variants, including error, info, required-action, disabled, empty, loading, and alternate identity-provider or authentication states where applicable.
- For Keycloakify or React-based renderers of FTL-based themes, map each official FTL template to the corresponding React page/component while preserving the official FTL contract. The output does not need to remain FreeMarker, but it must remain behaviorally and structurally equivalent to the official template.

## React Console Theme Workflow

- Do not require `admin` or `account` themes to implement every official FTL file. Admin/account completeness is judged against the official React console source contract, not `theme/base/<scope>/*.ftl`.
- Before creating or modifying an `admin` or `account` theme, read the official React console source for the target Keycloak version, including routes, shell layout, page modules, state model, permission model, i18n keys, and Admin REST API interaction patterns.
- For `admin` themes, use `@keycloak/keycloak-admin-ui`, official admin console source, or the repository's pinned Keycloak admin UI source as the behavioral reference when available.
- Treat admin/account FTL files only as bootstrapping shells, resource entry points, or compatibility references. Do not use them as the page coverage list.
- If the user explicitly names one or more admin/account pages or modules, implement those modules and the shared shell/state needed by them.
- If the user asks to create an admin/account theme but does not explicitly name a concrete page/module subset, add Storybook coverage for every implemented module that corresponds to the official React console modules present in the project.
- Admin/account completeness checks must ask whether the implementation covers the official React console shell, routes/pages, permission states, data states, form states, error states, and destructive-action flows.

## MUI Requirements

- Prefer current stable MUI packages and components already used by the repo, usually `@mui/material`, `@mui/icons-material`, and the repo's existing MUI styling pattern.
- Use MUI components instead of hand-rolled controls when an equivalent exists, especially for `Button`, `IconButton`, `TextField`, `Checkbox`, `Alert`, `Link`, `Typography`, `Stack`, `Box`, `FormControl`, and progress/loading states.
- Use MUI theme tokens for colors, spacing, radius, shadows, breakpoints, typography, and state colors. Avoid arbitrary hard-coded colors unless they define or extend the theme palette.
- Buttons and text must use MUI-native color semantics such as `primary`, `secondary`, `error`, `warning`, `info`, `success`, `text.primary`, `text.secondary`, and `action.*`.
- Use `@mui/icons-material` icons for icon buttons, adornments, alerts, and status affordances when an appropriate icon exists.
- Keep all buttons on the same page visually consistent in size. Choose one button size per page or form section (`small`, `medium`, or `large`) and apply it consistently to primary, secondary, and icon-bearing buttons unless there is a clear separate toolbar or dense list context.
- Keep button variants coherent: use a clear primary action, restrained secondary actions, and destructive styling only for destructive actions.
- Ensure text hierarchy follows MUI `Typography` variants and theme typography instead of custom one-off font sizes.

## Implementation Workflow

1. Inspect the existing theme structure before editing. Identify whether the project is plain Keycloak theme code, Keycloakify, or another wrapper.
2. Inventory existing theme scopes and their registration/configuration before changing files. Treat discovered scopes as user-owned unless the user explicitly asks to remove them.
3. Confirm the target theme scope from the request. Default new work to login, while preserving existing scopes.
4. Resolve the target Keycloak version and choose the correct official source contract: FTL for FTL-based scopes, React console source for admin/account scopes.
5. Decide coverage from that contract: implement named FTL files or console modules when specified; otherwise implement all official FTL files for FTL-based scopes or cover all implemented official console modules for admin/account scopes.
6. Reuse the repo's existing MUI theme provider, palette, component overrides, and layout conventions.
7. If adding dependencies or new MUI APIs, prefer the latest stable MUI major compatible with the repo. Check `package.json`, lockfiles, and current imports before changing versions.
8. Implement with MUI primitives first, then add local wrappers only when they remove real duplication or match an established local pattern.
9. Add or update Storybook for every supported theme scope and every implemented official FTL page or React console module in the change.
10. Validate responsive behavior for login pages: keyboard focus, password visibility, form errors, loading state, disabled state, and required Keycloak messages/links must remain accessible.

## Storybook Requirements

- Treat Storybook as required for any implemented or modified theme scope, including `login`, `admin`, `account`, `email`, or other user-requested scopes.
- Use the latest stable Storybook release channel for new setup and upgrades. For new projects, prefer the official `npm create storybook@latest` or package-manager equivalent. Do not use `@next`, alpha, beta, canary, or pinned old major versions unless the user explicitly asks for a prerelease or compatibility freeze.
- If the repository has Storybook already, extend its existing framework, builder, addons, preview decorators, theme providers, i18n providers, routing mocks, and file naming conventions, then upgrade it toward the latest stable version compatible with the repo. Prefer the official `npx storybook@latest upgrade` workflow when upgrading existing Storybook projects.
- When upgrading across multiple Storybook major versions, follow the official major-by-major upgrade path instead of jumping directly across several majors.
- Keep all `storybook` and `@storybook/*` packages on the same latest stable version range. Avoid mixed Storybook majors in `package.json` and lockfiles.
- Use the latest stable Storybook framework package that matches the application stack, such as `@storybook/react-vite` for React + Vite or the repo's equivalent framework package for Webpack/Next/etc.
- Add stories for every implemented official FTL page/template or React console module in the supported scope. Do not leave only a default or happy-path story.
- Base story coverage on the official Keycloak original theme behavior and visual states for that scope. Include equivalent page names, layout shell, form states, links, messages, validation errors, empty/loading/error states, disabled states, and required user actions.
- For login themes, cover all relevant official login templates present in the implementation, such as login, register, reset credentials, update password, OTP, WebAuthn, identity provider review/link flows, terms, error, info, and logout confirmation.
- For admin themes, cover the official React admin console shell and implemented modules with representative navigation, forms, tables, dialogs, filters, empty states, loading states, errors, permission-limited states, dirty forms, and destructive confirmations.
- Keep stories visually faithful to the official original Keycloak page structure while applying the MUI theme rules. Use official text/message keys or realistic fixtures instead of placeholder lorem ipsum.
- Ensure stories render with the same MUI palette, typography, spacing, icons, and button-size consistency as runtime pages.
- Include viewport-aware stories or testable variants for mobile and desktop when the page layout changes materially.
- Add Storybook scripts to `package.json` when missing, preserving the existing package manager. Common script names are `storybook` and `build-storybook`.

## Admin Storybook Layout

- Use this story title structure for admin themes: `Admin/<Domain>/<Page>/<State>`.
- Use these admin domains when applicable: `Shell`, `Realm`, `Clients`, `Users`, `Groups`, `Roles`, `IdentityProviders`, `Authentication`, `Sessions`, `Events`, and `SharedPatterns`.
- Each admin page/module story set must include these states when applicable: `Default`, `Loading`, `Empty`, `ValidationError`, `ApiError`, `PermissionDenied`, `DirtyForm`, `ConfirmDestructiveAction`, and `Mobile` when the layout changes materially on small screens.
- Admin Storybook global decorators must provide the MUI `ThemeProvider`, memory router, i18n provider, realm/admin context, permission matrix, mock Admin REST API handlers, and layout shell wrapper with sidebar, top bar, realm selector, and breadcrumbs.
- Admin stories must use semantic fixtures for realms, clients, users, groups, roles, identity providers, authentication flows, events, and sessions. Do not use lorem ipsum or meaningless placeholder data.
- Admin stories must preserve the official admin console information architecture, route context, permissions, data flow, and interaction states even when MUI replaces the visual component implementation.

## GitHub Actions Rules

- When editing workflows, update JavaScript-based actions to current stable major versions that run on Node24-compatible runtimes to avoid Node20 deprecation warnings.
- Verify current action majors from official action repositories or GitHub documentation before changing workflows; do not assume an old pinned example is still current.
- As a May 2026 baseline, prefer `actions/checkout@v6` and `actions/setup-node@v6` when compatible with the repository.
- Prefer `node-version: 24` or the project's supported active LTS/current version. Do not leave workflow setup pinned to Node20 unless the user explicitly requires Node20 compatibility.
- For self-hosted runners, account for GitHub runner compatibility. Node24 action runtime support requires sufficiently recent runner versions; if unknown, call this out. As a baseline, `actions/setup-node@v6` needs runner v2.327.1 or later, while `actions/checkout@v6` authenticated git commands from Docker container actions need runner v2.329.0 or later.
- Preserve existing package manager behavior (`npm`, `yarn`, or `pnpm`) and lockfile usage. Do not rewrite CI strategy unless needed to remove deprecation risk or support the theme build.

## Review Checklist

- Does the change preserve all existing Keycloak theme scopes, registrations, files, Storybook stories, and scripts unless the user explicitly requested removal?
- Is new theme-scope creation limited to login unless another scope was requested?
- If existing admin/account/email themes are present, were they fixed in place for whole-project requests or clearly reported as out of scope instead of deleted?
- Did the implementation read and follow the correct official source contract for the target Keycloak version and selected scope?
- For FTL-based themes, did it follow the official Keycloak FTL files, and if no concrete FTL subset was specified, were all official FTL files for the selected scope implemented?
- For admin/account themes, did it follow the official React console shell, routes/pages, permission states, data states, form states, error states, and destructive-action flows instead of judging completeness by FTL files?
- Does each implemented official FTL file or React console module have corresponding Storybook coverage?
- Does every implemented or modified theme scope include Storybook setup and complete stories?
- Do admin stories follow `Admin/<Domain>/<Page>/<State>` naming and include required decorators, semantic fixtures, and state coverage?
- Is Storybook installed or upgraded using the latest stable release channel, with matching `storybook` and `@storybook/*` package majors?
- Do the stories cover official Keycloak original page states rather than only custom happy paths?
- Are all visible controls using MUI components or repo-approved wrappers?
- Are button sizes consistent within each page or logical section?
- Are text, icon, and state colors sourced from the MUI theme palette?
- Are icons from `@mui/icons-material` where suitable?
- Are new controls from current stable MUI APIs rather than deprecated components?
- Are GitHub Actions free of Node20 action-runtime deprecation warnings where workflow files were touched?
