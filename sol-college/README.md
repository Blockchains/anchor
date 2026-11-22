# Sol.College campus integration branch (campus-only)

This directory exists only on the `sol-college` branch of `Blockchains/anchor`. It is never part of an upstream pull request.

| Role | Branch | Rule |
|---|---|---|
| Upstream-tracking baseline | `master` | Mirrors `otter-sec/anchor@master`. Synced with `gh repo sync Blockchains/anchor -b master`. No campus commits, never force-pushed. |
| Contribution branches | `fix/<issue-or-topic>` | Cut from the baseline. One upstream-ready change each. Source of the upstream PR. |
| Campus integration | `sol-college` | Baseline + merged contribution branches + this `sol-college/` directory. Protected (PR + 1 review, no force-push). |

## Integrated contribution branches

| Branch | Upstream PR | Status |
|---|---|---|
| `fix/program-error-send-transaction-logs` | https://github.com/otter-sec/anchor/pull/5354 | open upstream |

Live status: `contribution_traceability_register.csv` (updated daily by the PR monitor).

## Campus configuration

- Campus checks: `cd ts && yarn install --frozen-lockfile`, build `packages/borsh` and `packages/anchor-errors`, then `cd packages/anchor && npx jest tests`.
- Sol.College lessons consume `@anchor-lang/core` from this branch via a workspace link (`./setup-tests.sh`).

- No deployment secrets live in this fork; GitHub Actions are disabled on the fork.
- Devnet or local validator only. Never mainnet administrative identities.

## Rebuild after an upstream merge

1. `gh repo sync Blockchains/anchor -b master`
2. For each contribution branch: if the monitor reports it merged (upstream usually squash-merges, so detect by PR state, not by commit SHA), drop it; otherwise keep it.
3. Recreate `sol-college` from the new `master` head, merge the remaining contribution branches with `--no-ff`, and restore this directory from the previous `sol-college`.
4. Re-run the campus checks listed above before the branch-protection PR is approved.
