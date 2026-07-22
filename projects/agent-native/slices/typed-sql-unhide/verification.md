# Verification record — typed-sql-unhide

## Half A — delivered (S4-D1, 2026-07-04)

- **Engines PR:** https://github.com/prisma/prisma-engines/pull/5836 (open, base `main`) —
  commit `94c2c1f09a1`, 3 files +4/−3: `TypedSql` moved `hidden` → `active` in
  `psl/psl-core/src/common/preview_features.rs` (`ReactNative` stays hidden); the two tests
  embedding the rendered visible-features list updated
  (`psl/psl/tests/config/generators.rs`, `.../native_full_text_search_postgres/mysql.prisma`).
- Local validation: `CLICOLOR_FORCE=1 cargo test -p psl --features all` (1,297 pass) +
  `cargo test -p psl-core --all-features` + `cargo fmt --check`. Workspace-wide sweep found
  no other classification pins; CI (`make test-unit`) is the authoritative remaining
  surface, stated in the PR body.

## Half B — prep note (deferred past the publish boundary)

Execute after #5836 merges and a `@prisma/prisma-schema-wasm` dev version publishes:

1. `pnpm upgrade -r @prisma/prisma-schema-wasm@<dev version>` across the prisma/prisma
   workspace (pinned exact version, matching convention; cf. `scripts/bump-engines.ts`).
2. Snapshot fallout: prisma-side tests enumerating valid preview features — notably the
   `lintSchema`/validation territory in `packages/internals`; find exact files at bump time
   by searching snapshots for `strictUndefinedChecks`; each gains `typedSql` (bitflags
   iteration order places it between `strictUndefinedChecks` and `views`).
3. Acceptance: an invalid-preview-feature schema error lists `typedSql`; schemas already
   enabling `typedSql` behave identically.

Slice status: paused at the publish-dependency boundary by design (slice spec § Chosen
design); tracked on TML-2970.
