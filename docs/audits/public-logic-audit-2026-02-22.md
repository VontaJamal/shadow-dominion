# Public Logic Audit - 2026-02-22

## Repo
- VontaJamal/grimoire

## Scope
- Deep quality-control on existing public-facing logic only.
- No net-new product features.

## Baseline Snapshot
- Open PR count at start: 0
- Default branch: main
- Latest default-branch run (at start):
  - Agents Baseline Check: https://github.com/VontaJamal/grimoire/actions/runs/22282712362

## Public Surface Inventory
- README contract and published run commands
- Build/check scripts in package.json
- Next.js config + environment validation behavior
- Design submodule updater automation workflow
- Baseline governance workflows

## Command Matrix
| Check | Result | Notes |
|---|---|---|
| `npm run check` | PASS | Uses `eslint . && tsc --noEmit` |
| `npm run build` | PASS | Build passes with lint delegated to `check` |
| README relative link validation | PASS | No broken local targets |
| Updater workflow static review | PASS | Dynamic default branch + no-change + graceful fallback added |

## Findings Register
| Severity | Area | Repro | Status | Fix |
|---|---|---|---|---|
| P1 | Build/lint command contract | `npm run check` and `npm run build` failed when OAuth env vars were unset | Fixed | Added lifecycle-aware env validation skip for build/check/lint paths while preserving runtime validation gate |
| P1 | Updater workflow reliability | Updater always based on `main`, attempted PR on no-change runs, and could hard-fail when token lacked write | Fixed | Added dynamic default-branch resolution, submodule change detection, and graceful write-permission fallback |
| P1 | Default branch public-logic coverage gap | No default-branch workflow validated published command surface | Fixed | Added `Public Logic CI` on main push/PR for install, README link validation, check, and build |
| P1 | Submodule path integrity | Gitlink still pointed to `design/rinshari-ui` while `.gitmodules` declared `design/rinshari-eye`, breaking recursive checkout | Fixed | Renamed submodule gitlink path to `design/rinshari-eye` to match `.gitmodules` |

## Residual Risks / Follow-ups
- Dependency audit still reports high/critical vulnerabilities in transitive paths; triage in a dedicated dependency-risk wave to avoid accidental breaking upgrades.
- `eslint` warnings for anonymous default exports remain non-blocking and can be normalized in a style-only pass.

## Attestation
- This wave is maintenance and hardening only.
- No net-new product features were introduced.
