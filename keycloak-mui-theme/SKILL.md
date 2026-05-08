---
name: keycloak-mui-theme
description: Create, modify, or review Keycloak themes implemented with Material UI and latest stable Storybook coverage. Use when Codex works on Keycloak/Keycloakify theme code, especially login, admin, account, email, or other explicitly requested theme scopes; MUI component usage; button and typography consistency; MUI-native colors and icons; Storybook setup using the latest stable release channel; stories matching official Keycloak original pages and states; dependency choices for current MUI controls; or GitHub Actions updates that avoid Node20 deprecation warnings by using current action majors and Node24-compatible runtimes.
---

# Keycloak MUI Theme

## Overview

Use this skill to keep Keycloak theme work focused, MUI-native, and maintainable. Default to login theme only unless the user explicitly asks for another Keycloak theme type.

## Scope Rules

- Treat `login` as the only supported theme scope by default.
- Do not create or modify `account`, `admin`, `email`, or other Keycloak theme types unless the user names that scope or approves expanding beyond login.
- When the requested scope is ambiguous, implement the login theme path only and note the assumption.
- Keep non-login theme code isolated from login theme code when an expanded scope is requested.

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
2. Confirm the target theme scope from the request. Default to login.
3. Reuse the repo's existing MUI theme provider, palette, component overrides, and layout conventions.
4. If adding dependencies or new MUI APIs, prefer the latest stable MUI major compatible with the repo. Check `package.json`, lockfiles, and current imports before changing versions.
5. Implement with MUI primitives first, then add local wrappers only when they remove real duplication or match an established local pattern.
6. Add or update Storybook for every supported theme scope in the change.
7. Validate responsive behavior for login pages: keyboard focus, password visibility, form errors, loading state, disabled state, and required Keycloak messages/links must remain accessible.

## Storybook Requirements

- Treat Storybook as required for any implemented or modified theme scope, including `login`, `admin`, `account`, `email`, or other user-requested scopes.
- Use the latest stable Storybook release channel for new setup and upgrades. For new projects, prefer the official `npm create storybook@latest` or package-manager equivalent. Do not use `@next`, alpha, beta, canary, or pinned old major versions unless the user explicitly asks for a prerelease or compatibility freeze.
- If the repository has Storybook already, extend its existing framework, builder, addons, preview decorators, theme providers, i18n providers, routing mocks, and file naming conventions, then upgrade it toward the latest stable version compatible with the repo. Prefer the official `npx storybook@latest upgrade` workflow when upgrading existing Storybook projects.
- When upgrading across multiple Storybook major versions, follow the official major-by-major upgrade path instead of jumping directly across several majors.
- Keep all `storybook` and `@storybook/*` packages on the same latest stable version range. Avoid mixed Storybook majors in `package.json` and lockfiles.
- Use the latest stable Storybook framework package that matches the application stack, such as `@storybook/react-vite` for React + Vite or the repo's equivalent framework package for Webpack/Next/etc.
- Add stories for every implemented Keycloak page/template in the supported scope. Do not leave only a default or happy-path story.
- Base story coverage on the official Keycloak original theme behavior and visual states for that scope. Include equivalent page names, layout shell, form states, links, messages, validation errors, empty/loading/error states, disabled states, and required user actions.
- For login themes, cover all relevant official login templates present in the implementation, such as login, register, reset credentials, update password, OTP, WebAuthn, identity provider review/link flows, terms, error, info, and logout confirmation.
- For admin themes, cover the official admin console shell and implemented pages with representative navigation, forms, tables, dialogs, filters, empty states, loading states, errors, and permission-limited states.
- Keep stories visually faithful to the official original Keycloak page structure while applying the MUI theme rules. Use official text/message keys or realistic fixtures instead of placeholder lorem ipsum.
- Ensure stories render with the same MUI palette, typography, spacing, icons, and button-size consistency as runtime pages.
- Include viewport-aware stories or testable variants for mobile and desktop when the page layout changes materially.
- Add Storybook scripts to `package.json` when missing, preserving the existing package manager. Common script names are `storybook` and `build-storybook`.

## GitHub Actions Rules

- When editing workflows, update JavaScript-based actions to current stable major versions that run on Node24-compatible runtimes to avoid Node20 deprecation warnings.
- Verify current action majors from official action repositories or GitHub documentation before changing workflows; do not assume an old pinned example is still current.
- As a May 2026 baseline, prefer `actions/checkout@v6` and `actions/setup-node@v6` when compatible with the repository.
- Prefer `node-version: 24` or the project's supported active LTS/current version. Do not leave workflow setup pinned to Node20 unless the user explicitly requires Node20 compatibility.
- For self-hosted runners, account for GitHub runner compatibility. Node24 action runtime support requires sufficiently recent runner versions; if unknown, call this out. As a baseline, `actions/setup-node@v6` needs runner v2.327.1 or later, while `actions/checkout@v6` authenticated git commands from Docker container actions need runner v2.329.0 or later.
- Preserve existing package manager behavior (`npm`, `yarn`, or `pnpm`) and lockfile usage. Do not rewrite CI strategy unless needed to remove deprecation risk or support the theme build.

## Review Checklist

- Is the change limited to login theme unless another scope was requested?
- Does every implemented or modified theme scope include Storybook setup and complete stories?
- Is Storybook installed or upgraded using the latest stable release channel, with matching `storybook` and `@storybook/*` package majors?
- Do the stories cover official Keycloak original page states rather than only custom happy paths?
- Are all visible controls using MUI components or repo-approved wrappers?
- Are button sizes consistent within each page or logical section?
- Are text, icon, and state colors sourced from the MUI theme palette?
- Are icons from `@mui/icons-material` where suitable?
- Are new controls from current stable MUI APIs rather than deprecated components?
- Are GitHub Actions free of Node20 action-runtime deprecation warnings where workflow files were touched?
