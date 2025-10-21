# Repository Guidelines

Use this guide as the working agreement for contributing to Baileys. It mirrors how the maintainers keep the codebase stable and ready for release.

## Project Structure & Module Organization
- `src/` holds the TypeScript source; transport logic, socket state, and protocol helpers live under domain-focused folders (see `src/Signal`, `src/Utils`, `src/WAConnection`).
- `src/__tests__/` contains unit suites; end-to-end flows sit in `src/__tests__/e2e`.
- `Example/example.ts` is a runnable sandbox showcasing a basic bot flow; supporting media lives in `Media/`.
- `WAProto/` stores generated protocol buffers. Run `WAProto/GenerateStatics.sh` if the upstream schema changes. Supporting extraction utilities live in `proto-extract/`.

## Build, Test, and Development Commands
- `yarn build` compiles TypeScript with `tsc -P tsconfig.build.json`, emitting ESM and CJS-ready output in `lib/`.
- `yarn example` launches the sample client via `tsx` for quick manual verification.
- `yarn lint` runs `tsc` type checks and ESLint across `src`; `yarn lint:fix` chains Prettier before applying ESLint fixes.
- `yarn test` executes Jest unit suites (`*.test.ts`); `yarn test:e2e` runs the longer integration cases (`*.test-e2e.ts`).
- `yarn build:docs` produces API docs with TypeDoc; `yarn gen:protobuf` refreshes protobuf statics after schema updates.

## Coding Style & Naming Conventions
- TypeScript is the source of truth; prefer explicit exports from index modules to keep the public API intentional.
- Prettier enforces 2-space indentation and trailing commas where valid; run `yarn format` before submitting.
- Follow camelCase for functions/variables, PascalCase for classes, and SCREAMING_SNAKE_CASE for constants to match existing modules.
- ESLint (flat config plus `@whiskeysockets/eslint-config`) warns on redundant types and unused values; do not silence rules without discussion.

## Testing Guidelines
- Co-locate new tests under `src/__tests__/` mirroring the module path (`src/Foo/bar.ts` → `src/__tests__/Foo/bar.test.ts`).
- Aim to cover regressions with focused Jest assertions; add fixtures to `Media/` only when reusable.
- Annotate asynchronous expectations with explicit timeouts to avoid implicit waits; prefer `await expect(...).resolves`.
- Run `yarn test` (and `yarn test:e2e` when protocol/session logic is touched) before opening a pull request.

## Commit & Pull Request Guidelines
- Use Conventional Commit prefixes (`feat:`, `fix:`, `refactor:`, `docs:`) so release automation can produce accurate changelog notes.
- Keep commits logically scoped; include brief context in the body when behavior changes or migrations are required.
- Pull requests should describe the motivation, list observable changes, and mention any follow-up TODOs. Link GitHub issues and attach console logs or screenshots for UI-facing updates in the example client.
- Confirm CI checks locally when possible and call out anything skipped or flaky in the PR description.

## Security & Configuration Tips
- Develop on Node.js 20+; `engine-requirements.js` aborts installs when the version is incompatible.
- The library depends on `libsignal` via a Git URL—ensure SSH access is configured or switch to HTTPS when installing in CI.
- Never commit secrets or personal WhatsApp identifiers; use throwaway accounts in the example scripts and scrub captured payloads before sharing logs.
