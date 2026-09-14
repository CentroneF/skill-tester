# React Project Scaffold Implementation Plan

## Overview

Create a client-only React application foundation using Vite and TypeScript. The scaffold will provide a neutral, working welcome screen, a small plain-CSS baseline, documented local commands, and automated checks so future features can start from a dependable baseline.

## Current State Analysis

The repository contains only the Oracle change-lifecycle files; there is no Node project, application source, build configuration, test runner, or README. The change record for GitHub issue #2 is present at `context/changes/gh-2-oracle-new-tester-skill/change.md`, but the referenced issue body was unavailable during planning.

## Desired End State

A developer with a supported Node.js runtime can install dependencies, start a React development server, see a concise product-neutral welcome page, run linting and type checking, execute a render-level test, and create a production build. The project deliberately remains client-only: it has no routing, API layer, state library, deployment configuration, or design system.

### Key Discoveries:

- `context/changes/gh-2-oracle-new-tester-skill/change.md:3` identifies this as the active change; its status will be `planned` before implementation begins.
- The repository currently has no application files, so Vite's official `react-ts` template is the appropriate convention rather than an existing project pattern.
- Vite's current documentation requires Node.js 20.19+ or 22.12+; this prerequisite must be stated in the README and package metadata.
- Vite does not perform TypeScript type checking itself, so type checking must be exposed as an explicit package script alongside linting and build verification.
- Vitest can share Vite's React configuration, making it suitable for the requested smoke-test baseline.

## What We're NOT Doing

- Adding routes, server rendering, or an API/backend.
- Selecting a component library, design system, or Tailwind CSS.
- Adding application state management, authentication, persistence, or analytics.
- Adding end-to-end browser tests, CI/CD, hosting, or deployment configuration.
- Designing product-specific branding, content, or a dashboard layout.

## Implementation Approach

Generate the application in the repository root from Vite's React/TypeScript template, retaining only the toolchain structure that supports React Fast Refresh, TypeScript, and ESLint. Replace its demonstrative counter UI and template assets with a minimal welcome screen and two CSS files: one global baseline and one component-local layout. Add Vitest, React Testing Library, and a DOM test environment to exercise the welcome page through the same entry component used at runtime. Keep scripts explicit and documented so development, validation, and build workflows are discoverable without additional infrastructure.

## Phase 1: Scaffold the Application

### Overview

Create the Vite React/TypeScript project foundation and make every developer-facing quality command available from `package.json`.

### Changes Required:

#### 1. Vite project configuration and root metadata

**Files**: `package.json`, `package-lock.json`, `index.html`, `vite.config.ts`, `tsconfig.json`, `tsconfig.app.json`, `tsconfig.node.json`, `eslint.config.js`, `.gitignore`

**Intent**: Establish the Vite React/TypeScript application in the repository root with reproducible dependencies and standard development, build, preview, lint, and type-check workflows.

**Contract**: `package.json` must expose `dev`, `build`, `preview`, `lint`, and `typecheck` scripts; `typecheck` must validate both application and tooling TypeScript configurations without emitting artifacts. The project must document/enforce a Node.js baseline compatible with the selected Vite release.

#### 2. React bootstrap entrypoint

**Files**: `src/main.tsx`, `src/vite-env.d.ts`

**Intent**: Mount the root React component into Vite's root element and preserve Vite's client type declarations.

**Contract**: The browser entrypoint must render `App` under React Strict Mode and import the global stylesheet exactly once.

### Success Criteria:

#### Automated Verification:

- Dependencies install successfully with the repository lockfile: `npm ci`
- TypeScript validates application and tooling configuration: `npm run typecheck`
- ESLint completes without errors: `npm run lint`

#### Manual Verification:

- `npm run dev` serves the app locally without startup errors and supports a browser load at Vite's reported local URL.

**Implementation Note**: After completing this phase and all automated verification passes, pause here for manual confirmation from the human that the manual testing was successful before proceeding to the next phase.

---

## Phase 2: Create the Starter Experience

### Overview

Replace template demonstration content with a compact, brand-neutral welcome page and maintainable plain-CSS styling baseline.

### Changes Required:

#### 1. Welcome page component

**File**: `src/App.tsx`

**Intent**: Provide a minimal visible confirmation that the React application is mounted and ready for the first product feature, without embedding product assumptions.

**Contract**: `App` must render one semantic main content region containing a clear readiness heading and short explanatory text; it must not retain template counter state, logo imports, or sample interaction behavior.

#### 2. Global and component styles

**Files**: `src/index.css`, `src/App.css`

**Intent**: Establish a small accessible global baseline and centered welcome-page presentation using only plain CSS.

**Contract**: `index.css` owns document-level defaults (including typography, color, and box sizing); `App.css` owns the welcome-page layout. Styles must avoid dependencies on a utility framework, component library, or product brand assets.

#### 3. Remove Vite demonstration assets

**Files**: `src/assets/react.svg` (remove), any unused template asset references

**Intent**: Keep the starter repository focused on the selected neutral UI rather than shipping unused Vite template artifacts.

**Contract**: No application source file may import a removed asset, and the production build must remain asset-reference clean.

### Success Criteria:

#### Automated Verification:

- TypeScript continues to validate after the UI replacement: `npm run typecheck`
- ESLint continues to complete without errors: `npm run lint`
- Production bundle completes successfully: `npm run build`

#### Manual Verification:

- In a browser, the page visibly presents the readiness heading and explanatory text with a readable centered layout.
- The page remains usable and legible at a narrow mobile viewport and a typical desktop viewport.

**Implementation Note**: After completing this phase and all automated verification passes, pause here for manual confirmation from the human that the manual testing was successful before proceeding to the next phase.

---

## Phase 3: Establish Verification and Handoff

### Overview

Add component-test support and concise project documentation so contributors can validate and extend the scaffold consistently.

### Changes Required:

#### 1. Test runner configuration and setup

**Files**: `vite.config.ts`, `src/test/setup.ts`, `package.json`

**Intent**: Configure Vitest with a DOM-like environment and Testing Library cleanup so React components can be tested through user-visible output.

**Contract**: `npm test` (or an equivalent documented `test` script) must execute once and exit with a meaningful status for CI; the Vite configuration must retain the React plugin while declaring test settings. The shared setup must register the assertion and cleanup behavior required by the chosen test utilities.

#### 2. Welcome-page smoke test

**File**: `src/App.test.tsx`

**Intent**: Verify that the starter application renders its primary user-facing readiness message.

**Contract**: The test must render `App` with React Testing Library and assert the visible heading by its accessible role and name, rather than depending on implementation-only class names or markup structure.

#### 3. Contributor documentation

**File**: `README.md`

**Intent**: Give the first contributor a concise setup, command, and scope reference for the otherwise empty project.

**Contract**: The README must state the Node.js prerequisite, installation command, development command, test/lint/type-check/build commands, and the intentionally deferred capabilities (routing, API/backend, UI library, deployment).

### Success Criteria:

#### Automated Verification:

- The welcome-page test passes in one-shot mode: `npm test`
- TypeScript validates all configured source and tooling files: `npm run typecheck`
- Linting passes for source and test files: `npm run lint`
- The production build completes: `npm run build`

#### Manual Verification:

- A new contributor can follow the README from clone through local development without undocumented setup steps.
- The production output can be inspected through `npm run preview` and displays the same welcome page.

**Implementation Note**: After completing this phase and all automated verification passes, pause here for manual confirmation from the human that the manual testing was successful before concluding the change.

## Testing Strategy

### Unit Tests:

- Render `App` using React Testing Library.
- Assert the readiness heading by accessible role and visible name.
- Keep the suite to a smoke test until the app has user behavior or domain logic worth exercising.

### Integration Tests:

- No end-to-end test suite is in scope for this scaffold.
- Treat `npm run build` followed by `npm run preview` as the initial integration check between Vite's production output and the browser.

### Manual Testing Steps:

1. Install dependencies with the documented command on a supported Node.js version.
2. Start the development server and open its local URL in a desktop browser.
3. Confirm the welcome page is readable at both narrow and desktop viewport widths.
4. Build and preview the production output, then confirm it renders the same page.

## Performance Considerations

The first render is static and dependency-light; no performance optimization, bundle splitting, or loading-state behavior is warranted. Retaining only the base Vite/React dependencies and removing unused template assets keeps the initial bundle and maintenance footprint small.

## Migration Notes

No data or system migration applies because this repository has no existing application. Future additions such as routing, API access, state management, a UI library, or deployment should be planned as separate changes rather than inferred into this scaffold.

## References

- Active change record: `context/changes/gh-2-oracle-new-tester-skill/change.md`
- Vite React TypeScript template and Node.js prerequisite: [Vite Getting Started](https://vite.dev/guide/)
- Vite build and TypeScript behavior: [Vite Features](https://vite.dev/guide/features), [Vite Building for Production](https://vite.dev/guide/build)
- Shared Vite/Vitest configuration: [Vitest Getting Started](https://vitest.dev/guide/)

## Progress

> Convention: `- [ ]` pending, `- [x]` done. Append ` — <commit sha>` when a step lands. Do not rename step titles. See `references/progress-format.md`.

### Phase 1: Scaffold the Application

#### Automated

- [ ] 1.1 Dependencies install successfully with the repository lockfile: `npm ci`
- [ ] 1.2 TypeScript validates application and tooling configuration: `npm run typecheck`
- [ ] 1.3 ESLint completes without errors: `npm run lint`

#### Manual

- [ ] 1.4 Development server starts and serves the app locally without errors

### Phase 2: Create the Starter Experience

#### Automated

- [ ] 2.1 TypeScript continues to validate after the UI replacement: `npm run typecheck`
- [ ] 2.2 ESLint continues to complete without errors: `npm run lint`
- [ ] 2.3 Production bundle completes successfully: `npm run build`

#### Manual

- [ ] 2.4 Welcome page presents readable readiness content in a browser
- [ ] 2.5 Welcome page remains usable at narrow and desktop viewport widths

### Phase 3: Establish Verification and Handoff

#### Automated

- [ ] 3.1 Welcome-page test passes in one-shot mode: `npm test`
- [ ] 3.2 TypeScript validates all configured source and tooling files: `npm run typecheck`
- [ ] 3.3 Linting passes for source and test files: `npm run lint`
- [ ] 3.4 Production build completes: `npm run build`

#### Manual

- [ ] 3.5 New contributor can follow the README through local development without undocumented steps
- [ ] 3.6 Production preview displays the same welcome page
