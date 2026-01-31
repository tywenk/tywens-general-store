# JavaScript/TypeScript Stack Profile

## Import Patterns
- Check for mixed CommonJS (`require()`) and ESM (`import/export`) — indicates incomplete migration
- Look for barrel files (`index.ts` re-exporting) — can hide circular deps and bloat bundles
- Check for path aliases (`@/`, `~/`) vs relative paths — inconsistency signals different eras
- Look for dynamic `require()` calls — hard to analyze statically

## Package Manager
- `package-lock.json` → npm
- `yarn.lock` → yarn (check for `.yarnrc.yml` = yarn berry vs classic)
- `pnpm-lock.yaml` → pnpm
- Multiple lock files = migration in progress or inconsistency
- Check `engines` field in package.json for Node version constraints

## Framework Signals
- `next.config.*` → Next.js (check for `pages/` vs `app/` router migration)
- `angular.json` → Angular (check version in package.json for era)
- `vue.config.*` or `vite.config.*` → Vue
- `gatsby-config.*` → Gatsby
- Express/Fastify/Koa/Hapi → check for consistent middleware patterns
- React class components vs functional components with hooks → era indicator

## Legacy Indicators
- jQuery alongside React/Vue/Angular → incremental migration
- Webpack config alongside Vite → build tool migration
- Babel config with modern TypeScript → may be unnecessary
- `var` declarations → very old code or generated code
- Callback patterns alongside async/await → incomplete modernization
- `tslint.json` alongside `eslint` → linter migration

## Configuration Surfaces
- `tsconfig.json` (check `strict`, `target`, `module` settings)
- `.eslintrc.*` / `eslint.config.*` (flat config migration)
- `.babelrc` / `babel.config.*`
- `webpack.config.*` / `vite.config.*` / `rollup.config.*`
- `.env*` files (check for secrets, inconsistency across environments)
- `jest.config.*` / `vitest.config.*`

## Testing Conventions
- `__tests__/` directories vs `*.test.ts` co-located — check consistency
- Jest vs Vitest vs Mocha — check for migration between them
- Cypress / Playwright / Puppeteer for E2E — check for overlap
- Check test utility patterns (custom render wrappers, test factories)

## Common Migration Patterns
- CJS → ESM: Look for `"type": "module"` in package.json
- JavaScript → TypeScript: Look for mixed `.js`/`.ts` files, `allowJs` in tsconfig
- Class components → Hooks: Look for `extends Component` alongside `useState`
- REST → GraphQL: Look for both patterns coexisting
- Monolith → Monorepo: Look for `workspaces` in package.json or `lerna.json`
