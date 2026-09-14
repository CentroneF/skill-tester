# React Project Scaffold — Plan Brief

> Full plan: `context/changes/gh-2-oracle-new-tester-skill/plan.md`

## What & Why

Create a clean React and TypeScript application foundation in an otherwise empty repository. It provides an immediately verifiable welcome page and a dependable developer workflow without guessing at the product, backend, or design system that will come later.

## Starting Point

The repository currently contains only Oracle change-management artifacts; it has no frontend source, package manifest, build tooling, tests, or contributor documentation. The active record is `context/changes/gh-2-oracle-new-tester-skill/change.md`.

## Desired End State

A contributor can install dependencies on a supported Node.js runtime, launch a Vite-powered React app, and see a concise welcome page. They can also run linting, type checking, a render-level test, and a production build with documented commands.

## Key Decisions Made

| Decision | Choice | Why (1 sentence) |
| --- | --- | --- |
| Application foundation | Vite + React + TypeScript | A lean, current scaffold provides fast local development and typed source without the extra architecture of a full-stack framework. |
| Styling | Plain CSS baseline | It creates minimal, maintainable visual conventions without committing the project to a utility framework or component library. |
| Starter screen | Neutral welcome page | It visibly proves the application works while avoiding product or branding assumptions. |
| Quality baseline | Vitest + React Testing Library, lint, type check, and build | A render-level smoke test and executable static checks catch setup regressions without an unjustified end-to-end suite. |
| Scope boundary | Client-only, documented foundation | Routing, APIs, state management, UI libraries, and deployment remain intentional future decisions. |

## Scope

**In scope:**

- Vite React/TypeScript project files and npm scripts.
- Neutral welcome page with global and component-local CSS.
- ESLint, explicit type checking, Vitest, React Testing Library, and one accessibility-oriented smoke test.
- README, lockfile, ignore rules, and Node.js prerequisite.

**Out of scope:**

- Routing, APIs/backend, authentication, persistence, state library, analytics, or server rendering.
- A component library, Tailwind, a product design, or branded assets.
- End-to-end automation, CI/CD, hosting, and deployment setup.

## Architecture / Approach

Vite's root `index.html` loads `src/main.tsx`, which mounts `App`. `App` imports its local layout styles while `main.tsx` imports global defaults. Vite powers development and production builds; Vitest shares its configuration to render `App` in a DOM-like test environment.

## Phases at a Glance

| Phase | What it delivers | Key risk |
| --- | --- | --- |
| 1. Scaffold the Application | Vite/React/TypeScript configuration, scripts, and bootstrap entrypoint | Compatibility with the selected Vite release's Node.js requirement. |
| 2. Create the Starter Experience | Neutral welcome UI, plain-CSS baseline, and removal of template artifacts | Accidentally retaining template-specific content or asset imports. |
| 3. Establish Verification and Handoff | Component smoke test and contributor README | Keeping the test configuration small while ensuring it runs reliably in one-shot mode. |

**Prerequisites:** Node.js 20.19+ or 22.12+ and npm.
**Estimated effort:** ~1 focused implementation session across 3 checkpoints.

## Open Risks & Assumptions

- The GitHub issue body could not be retrieved; this plan treats the supplied “create a react project scaffold” request as authoritative.
- The repository root is intentionally the application root, rather than a monorepo or nested frontend directory.
- Dependency versions should be chosen from the current official Vite template at implementation time, then locked in `package-lock.json`.

## Success Criteria (Summary)

- A developer can install dependencies and run the React app locally using documented commands.
- The neutral welcome screen is accessible, responsive enough for narrow and desktop viewports, and builds for production.
- Linting, type checking, the component smoke test, and the production build all pass.
