# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

All commands run from within an app directory (`apps/react-vite`, `apps/nextjs-app`, or `apps/nextjs-pages`):

```bash
yarn dev          # start dev server
yarn build        # type-check + build
yarn test         # run Vitest (unit/integration)
yarn test:e2e     # run Playwright E2E tests
yarn lint         # ESLint
yarn check-types  # tsc --noEmit
yarn generate     # Plop code generators
yarn storybook    # Storybook on port 6006
```

Run a single test file: `yarn test src/features/discussions/components/discussion-view.test.tsx`

## Architecture

This is a monorepo with three reference implementations of the same app (a team collaboration platform with discussions, comments, and RBAC). The primary reference is `apps/react-vite`.

**Dependency direction:** `shared (components/hooks/utils) → features → app`. Features must not import from each other.

```
src/
├── app/         # router, providers, root layout
├── components/  # shared UI (Radix UI + Tailwind, ShadCN pattern — copied in, not installed)
├── config/      # env vars
├── features/    # self-contained feature modules
├── hooks/       # shared hooks
├── lib/         # configured library wrappers (react-query client, auth)
├── testing/     # test utils, MSW handlers
├── types/       # shared TS types
└── utils/       # shared utilities
```

Each feature is structured as:
```
features/<name>/
├── api/         # fetcher functions + TanStack Query hooks (one file per endpoint)
├── components/
├── hooks/
├── stores/      # Zustand stores
├── types/
└── utils/
```

## Key Patterns

**API layer:** Every endpoint has a plain fetcher function and a separate React Query hook:
```typescript
export const getDiscussions = (params: Params): Promise<Discussion[]> =>
  api.get('/discussions', { params });

export const useDiscussions = (params: Params) =>
  useQuery({ queryKey: ['discussions', params], queryFn: () => getDiscussions(params) });
```

**Imports:** Always use `@/` alias for `src/` imports.

**Testing:** Integration tests are the primary focus. Use MSW for API mocking — do not mock `fetch` or `axios` directly. Test behavior and user interactions, not implementation details.

**State:** Local state → lifted state → Zustand (global UI state only). React Query owns all server state.

**Forms:** React Hook Form + Zod schemas.

**Git hooks:** Pre-commit runs ESLint + TS check; pre-push runs tests. Do not skip with `--no-verify`.
