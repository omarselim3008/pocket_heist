# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

Use `yarn` as the package manager (not `npm`).

```bash
yarn dev        # Start development server at localhost:3000
yarn build      # Production build
yarn lint       # Run ESLint
yarn test       # Run all tests (Vitest, watch mode)
yarn test --run # Run tests once (CI mode)
```

To run a single test file:
```bash
yarn test tests/components/Navbar.test.tsx --run
```

## Architecture

### Routing

The app uses **Next.js App Router** with two route groups that share no layouts with each other:

- `app/(public)/` — Unauthenticated pages (home splash, login, signup, preview). The layout wraps children in `<main className="public">`.
- `app/(dashboard)/` — Auth-protected pages under `/heists`. The layout injects the `<Navbar />` above a `<main>` block.

The home page (`/`) is intended as a redirect gateway: logged-in users go to `/heists`, unauthenticated users go to `/login`. This redirect logic is not yet implemented.

### Styling

Tailwind CSS 4 is used throughout. Custom theme tokens are defined via `@theme` in `app/globals.css` and are available as Tailwind utilities (e.g. `text-primary`, `bg-dark`, `text-body`). CSS Modules are used for component-scoped styles (e.g. `Navbar.module.css`).

Key theme tokens:
- `--color-primary`: `#C27AFF` (purple)
- `--color-secondary`: `#FB64B6` (pink)
- `--color-dark/light/lighter`: dark background scale
- `--color-body`: `#99A1AF` (default text)

Global layout utility classes (`.center-content`, `.page-content`, `.form-title`) are defined in `globals.css`.

### Components

Components live in `components/<ComponentName>/` with an `index.ts` barrel export. Import them via the `@/` path alias (e.g. `import Navbar from "@/components/Navbar"`).

### Testing

Tests live in `tests/` mirroring the source structure (e.g. `tests/components/`). Vitest is configured with `jsdom` environment, globals enabled, and `@testing-library/jest-dom` matchers set up in `vitest.setup.ts`. The `@/` path alias works in tests via `vite-tsconfig-paths`.
