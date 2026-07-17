# javascript-action

JavaScript GitHub Action repository focused on build, test, security scan, and
snapshot publishing workflows.

Prerequisites

- Node.js (recommended: >=24 — see `.node-version`)
- npm

Quick commands

- Install dependencies: `npm ci`
- Format (write): `npm run format:write`
- Format (check): `npm run format:check`
- Lint: `npm run lint`
- Test: `npm test`
- Package/bundle: `npm run package`
- Full workflow locally: `npm run all`

Notes:

- CI runs these on GitHub Actions; running locally mirrors the CI checks.
- Use `npm ci` in CI or when `package-lock.json` is present; use `npm install`
  only if you need to update dependencies.

What this repository contains

- `src/` — source implementation of the action
- `__tests__/`, `__fixtures__/` — Jest tests and fixtures
- `action.yml` — action metadata used when publishing/consuming this action
- `rollup.config.js` — bundle configuration used by `npm run package`
- `package.json` / `package-lock.json` — scripts and dependencies
- `.github/workflows/ci.yml` — main CI: format check, lint, test, audit, package
- `.github/workflows/codeql-analysis.yml` — CodeQL security analysis
- `jest.config.js`, `eslint.config.mjs`, `.prettierrc.yml` — test and lint
  config

CI behavior

- The CI workflow (`.github/workflows/ci.yml`) runs on pushes to `main` and pull
  requests. It performs:
  - `npm ci`
  - format check (`npm run format:check`)
  - lint (`npm run lint`)
  - tests (`npm test`)
  - dependency audit (`npm audit --audit-level=moderate`)
  - package bundle (`npm.run package`)

Security scanning

- CodeQL analysis is configured in `.github/workflows/codeql-analysis.yml` and
  scans the `src/` tree for security issues on PRs and scheduled runs.

Snapshot publishing

- This repository does not auto-publish snapshots by default. To publish a
  snapshot to npm from your machine:

```bash
# set your npm auth token locally (or use environment variables)
npm set //registry.npmjs.org/:_authToken=${NPM_TOKEN}
npm publish --tag snapshot --access public
```

- To enable CI-based snapshot publishing, add an `NPM_TOKEN` secret and add a
  `publish_snapshot` job to `.github/workflows/ci.yml` (or re-enable the
  previously-used publish job).

Development notes

- Keep `package-lock.json` — CI uses `npm ci` for reproducible installs.
- Keep `src/`, `__tests__/`, and `__fixtures__/` if you want automated testing
  and CI validation.
- `dist/` and `badges/` are intentionally not committed; build artifacts are
  created in CI or by running `npm run package` locally.

If you want me to:

- add an automated publish job to CI (requires `NPM_TOKEN`),
- add an additional security scanner (Snyk/Checkov), or
- run and report test results locally (you'll need Node/npm installed here), let
  me know which and I'll update the repository accordingly.
