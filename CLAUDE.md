# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

DmpAdmin is a Vue 3 admin template based on the SoybeanAdmin project. It uses a pnpm monorepo architecture with Vue 3, Vite 7, TypeScript, Pinia, Naive UI, and UnoCSS.

## Development Commands

```bash
pnpm dev              # Start dev server (test mode, port 9527)
pnpm dev:prod         # Start dev server (prod mode)
pnpm build            # Build for production
pnpm build:test       # Build for test environment
pnpm preview          # Preview production build
pnpm lint             # Run oxlint + eslint with --fix
pnpm fmt              # Run oxfmt formatter
pnpm typecheck        # Run vue-tsc type checking
pnpm commit           # Generate conventional commit message
pnpm commit:zh        # Generate conventional commit message (Chinese)
pnpm gen-route        # Generate route declarations
pnpm release          # Release new version
pnpm cleanup          # Cleanup build artifacts
```

## Architecture

### Monorepo Structure

- `/packages/` - Shared internal packages:
  - `alova` - Alova-based HTTP client with mock support
  - `axios` - Axios wrapper with interceptors, error handling, and token management
  - `color` - Color manipulation utilities
  - `hooks` - Composable hooks (useBoolean, useLoading, useTable, etc.)
  - `materials` - UI components and layout materials
  - `scripts` - CLI tools (git-commit, gen-route, release, cleanup)
  - `uno-preset` - UnoCSS preset with theme variables
  - `utils` - Common utilities (crypto, storage, nanoid)

### Main Application (`/src/`)

- `/src/main.ts` - App entry point, sets up store, router, i18n
- `/src/router/` - Vue Router with dynamic route guards and builtin routes
- `/src/store/` - Pinia store with modules: app, auth, route, tab, theme
- `/src/views/` - Page components
- `/src/components/` - Reusable components (common, custom, advanced)
- `/src/layouts/` - Layout system with multiple menu modes (vertical, horizontal, hybrid)
- `/src/service/` - API layer with request handling
- `/src/hooks/` - App-specific hooks (business/, common/)
- `/src/locales/` - i18n configuration and language files
- `/src/theme/` - Theme configuration and variables
- `/src/utils/` - Utility functions
- `/src/constants/` - App constants
- `/src/enum/` - TypeScript enums

### Key Conventions

- **Path aliases**: `@/*` maps to `./src/*`, `~/*` maps to `./*`
- **Component naming**: PascalCase for component names in templates
- **Icon naming**: `icon-` prefix for icons (e.g., `icon-mdi:home`)
- **API layer**: Uses `@sa/axios` package with standardized response handling
- **Route home**: Configured via `VITE_ROUTE_HOME` (default: `home`)
- **Auth modes**: `static` (frontend routes) or `dynamic` (backend routes) via `VITE_AUTH_ROUTE_MODE`

### Build Configuration

- Vite config in `/vite.config.ts` loads mode-based env files (`.env`, `.env.test`, `.env.prod`)
- UnoCSS for atomic CSS with `presetWind3` and custom `presetDmpAdmin`
- ESLint extends `@soybeanjs/eslint-config-vue`
- Simple-git-hooks enforce pre-commit (typecheck, lint, fmt) and commit-msg validation

### Environment Variables

Key variables in `.env`:

- `VITE_BASE_URL` - App base URL (default: `/`)
- `VITE_ROUTER_HISTORY_MODE` - hash | history | memory
- `VITE_SERVICE_SUCCESS_CODE` - Backend success code (default: `0000`)
- `VITE_SERVICE_MODAL_LOGOUT_CODES` - Codes triggering modal logout
- `VITE_SERVICE_EXPIRED_TOKEN_CODES` - Codes triggering token refresh
- `VITE_STATIC_SUPER_ROLE` - Super role for static route mode
