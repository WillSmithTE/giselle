# Continuity ledger (per-branch)

## Human intent (must not be overwritten)

- Verify whether the reported SDK parser edge case is a real bug and fix it minimally.
- Preserve graceful handling for failed/cancelled tasks that may omit `outputType`.
- Keep changes limited to the bug fix only.

## Goal (incl. success criteria)

- Confirm the parser regression and restore non-throwing behavior for task responses with `steps` but missing full result shape.
- Ensure `runAndWait()` can return task status for non-completed terminal tasks instead of crashing.

## Constraints/Assumptions

- Make the smallest possible change in `packages/sdk/src/sdk.ts`.
- Do not introduce unrelated refactors or behavior changes.

## Key decisions

- Treat full-result parsing as requiring both `steps` and `outputs` arrays, matching prior fallback behavior.

## State

- Bug reproduced by inspection: `parseTaskResponseJson` threw for task payloads with `steps` but missing/invalid `outputType`.
- Minimal fix implemented and covered by tests in `packages/sdk`.

## Done

- Created branch ledger and captured bug-fix intent.
- Updated `parseTaskResponseJson` to fall back to `TaskWithStatus` when full-result discriminator (`outputType`) is absent/unknown.
- Added regression test for `runAndWait()` failed-task flow where `includeGenerations=1` response omits `outputType`.
- Ran `pnpm install` (workspace dependencies), `pnpm build-sdk`, and `pnpm -F @giselles-ai/sdk test` (passing).

## Now

- Prepare commit/push and bugfix report.

## Next

- Commit and push the single bugfix commit.

## Open questions (UNCONFIRMED if needed)

- None.

## Working set (files/ids/commands)

- `packages/sdk/src/sdk.ts`
- `packages/sdk/src/sdk.test.ts`
- `f88545d3-a0cb-4224-aa60-3f97fa27bbf9`

