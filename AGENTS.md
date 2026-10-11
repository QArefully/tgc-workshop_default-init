# Repository Guidelines

## Project Structure & Module Organization

QArefully Materials Exchange is a local wholesale demo for QA education, organized as npm workspaces:

- `apps/web/src/`: React frontend; feature screens in `features/`, reusable UI in `components/`, static assets in `apps/web/public/`.
- `apps/api/src/`: Fastify API; domain workflows in `features/`, SQLite migrations and seeds in `db/`. Integration tests live in `apps/api/test/`.
- `packages/contracts/`, `packages/catalog/`, `packages/localisation/`: shared transport schemas, canonical product data, and translations/formatters.
- `e2e/`: Playwright specs and page objects; `scripts/`: validation tools; `workshop/`: exercise materials.

Keep shared packages independent of app-private code. The web app consumes shared API contracts; never import API source into it.

## Build, Test, and Development Commands

Use Node.js 22.x and run commands from the repository root:

- `npm ci`: install locked dependencies and build shared packages.
- `npm run dev`: seed SQLite and start web (`127.0.0.1:5173`) plus API (`127.0.0.1:3001`).
- `npm run build --workspaces --if-present`: build all workspaces.
- `npm test`: run package, unit, and integration suites.
- `npm run test:e2e`: run Chromium browser tests; install it first with `npx playwright install chromium`.
- `npm run verify`: check formatting, localisation, types, lint, tests, and builds. Run browser tests separately.
- `npm run format:fix`: apply Prettier formatting.

## Coding Style & Naming Conventions

Use strict TypeScript, two-space indentation, LF endings, semicolons, single quotes, and trailing commas. Prettier uses a 100-character print width; ESLint enforces typed rules and import boundaries. Follow existing names: PascalCase React components (`ProductCard.tsx`), camelCase functions/modules, and `use`-prefixed hooks. Keep translated copy and country-aware formatting in the localisation system.

## Testing Guidelines

Frontend tests use Vitest, jsdom, and React Testing Library; API/shared-package tests use Node's test runner through `tsx`. Colocate unit tests as `*.test.ts`/`*.test.tsx`; name integration tests `*.integration.test.ts`/`*.integration.test.tsx`. Browser specs use `*.spec.ts`, accessible locators, and web-first assertions. Keep assertions out of page objects. No numeric coverage threshold is configured; cover changed behavior and regressions.

## Commit & Pull Request Guidelines

History contains only `Initial commit`, so no established message convention exists. Use concise, imperative subjects. Keep PRs focused; describe behavior changes, link relevant issues, report validation commands/results, and include screenshots for UI changes.

## Configuration & Data

The default database is `apps/api/data/shop.db`; override with `SHOP_DB_PATH`. `npm run reset` deletes local data and reseeds it. Keep database files and secrets untracked. Payments and external workflows are simulated; persisted monetary values remain integer GBP pence.
