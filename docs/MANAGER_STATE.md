# Round 2 — Existing Codebase Manager Experiment

Status:
ACTIVE

Current phase:
Batch 2 implementation in progress / Manager review pending.
Current gate: Batch 2 implementation in progress / Manager review pending.

Experiment repository:
r7-1505165673/hardware-assets-inventory-tool

Upstream repository:
mikhailbahdashych/hardware-assets-inventory-tool

Frozen upstream baseline:
db7e6ffccc58fe6a47aaa0cf489eeb0225f9c6bb

Selection result: PASS

Takeover Baseline Audit result: PASS WITH ISSUES

## Product summary

A self-hosted hardware inventory system for IT teams. It tracks assets, employees, ownership history, configurable statuses and workflows, roles and permissions, attachments, activity, dashboards, imports, optional email, and two-factor authentication. Demo mode uses fictional data and the application supports SQLite by default.

## Architecture summary

- npm workspaces monorepo
- `apps/web`: React + Vite SPA
- `apps/api`: Fastify API
- `packages/shared`: shared domain vocabulary, RBAC, and Zod contracts
- `e2e`: Playwright
- Default local persistence: SQLite
- PostgreSQL and S3 exist but are OUT OF SCOPE for the Round 2 experiment

## Important domain invariant

Who holds an asset is represented by assignments / ownership history, not by storing the holder directly on the asset.

## Inherited baseline verification

The frozen upstream baseline had a successful GitHub Actions CI run covering the repository's existing gates. These are inherited results from the exact baseline, not Batch 0 reruns.

Known baseline test evidence:

- 482 API tests passed
- 329 Web tests passed
- 166 Shared tests passed
- 977 total unit/integration tests passed
- 49 Playwright E2E tests passed

## Known issues / risks

- Baseline npm install/audit evidence reported 4 moderate vulnerabilities.
- Production web build emitted a >500 kB chunk warning.
- Upstream has dependency-security follow-ups after the frozen baseline.
- Upstream fast-uri security update PR #42 is known to have green CI.
- Upstream Fastify 5.12.1 security update PR #43 is known to fail API typecheck.
- That Fastify failure intersects with the existing `trustProxy` security design and is not a blind dependency bump.
- Main branch protection is not relied upon as the experiment control; process discipline is mandatory.

## Experimental confound

The upstream repository contains extensive `CLAUDE.md` and agent-oriented engineering instructions. Round 2 deliberately keeps them as part of the existing codebase; ChatGPT Manager must still make independent decisions before delegating execution.

## Round 2 scope boundaries

Allowed: local development, fictional demo data, SQLite, and bounded source/test/doc changes approved by Manager.

Not allowed: production systems, real user or company asset data, real credentials or secrets, payment systems, AWS apply, Terraform deployment, production PostgreSQL, production S3, real SMTP, or unrelated repositories.

## Governance

Every implementation change follows:

feature branch → commit → PR → independent ChatGPT repository/diff review → PASS or FIX REQUIRED → re-review if fixed → explicit merge authorization.

Codex must never merge on its own.

## Batch 0 merged state

Current main after Batch 0 merge:
`8f68af49e1e8172c496e3785b41f2f44c25e6a58`

Batch 0 PR: #1

Batch 0 reviewed PR head:
`0677cf5458ff70c4b7f57cf0b616ea046f321dee`

Batch 0 merge commit:
`8f68af49e1e8172c496e3785b41f2f44c25e6a58`

Batch 0 final result: PASS

Review history:

- Initial Manager Review: FIX REQUIRED
- Fix 01 commit: `0677cf5458ff70c4b7f57cf0b616ea046f321dee`
- Manager Re-review: PASS
- Merge authorization: granted only for that exact head
- PR #1: merged

## Fork CI state

Inherited baseline evidence: the frozen upstream baseline had successful GitHub Actions CI and recorded 482 API, 329 Web, 166 Shared, 977 total unit/integration, and 49 Playwright E2E tests.

Fork CI Gate: PASS. Verified PR #3, head 0c21949bdef40c3ced59ddc5b5c31989dc815cb6, GitHub Actions run 34459287398. Verified jobs: ci PASS; api-tests-postgres PASS; image PASS; terraform PASS. Main ci passed lint, repository-wide format check, typecheck, unit tests, build, Playwright browser install, and E2E.

Batch 1 technical implementation: VALIDATED / PASS.

Current stage: Batch 2 implementation in progress / Manager review pending.

Fresh npm ci advisory observation: 9 vulnerabilities (7 moderate, 2 high). Dependency remediation: NOT part of this state sync.

## Batch 1 state

Batch 1: CLOSED / PASS.

PR: #4

Authorized final head: 50630a736fddc8971a569eb17a1e58942aac539b

Merge commit: d57a9a3d44bf401e186233280df32eb11531e75d

Post-merge main CI: 34473410139 — PASS.

Batch 1 technical implementation: VALIDATED / PASS.

PR: #4

Reviewed implementation head: e329cf149c2dae74e752f7c60abae7797cba505e

Validated CI run: 34469480917

Validated dependency change:

- fast-uri 3.1.5 → 3.1.7
- fast-uri 4.1.2 → 4.1.4

Dependency type: transitive / indirect

Validated GitHub CI:

- ci PASS
- api-tests-postgres PASS
- image PASS
- terraform PASS

Regression evidence:

- API tests: 482 passed
- Web tests: 329 passed
- Shared tests: 166 passed
- Total unit/integration: 977 passed
- Playwright E2E: 49 passed

Current audit observation: 8 vulnerabilities

- 7 moderate
- 1 high

Remaining unrelated vulnerability remediation: OUT OF SCOPE for Batch 1.

Important recovery rule:
Batch 1 technical implementation is validated. Live PR / merge status must be derived from GitHub / Git history. Before Batch 2 implementation, Manager must verify:

1. PR #4 was merged from the authorized final head.
2. Post-merge main CI passed.

If both conditions are true, Batch 1 should be treated as CLOSED / PASS without creating another state-only PR merely to flip that checkpoint.

Current next stage: Batch 2 implementation in progress / Manager review pending.

Batch 2 implementation: IN PROGRESS / Manager review pending.

## Batch 2 state

Implementation: IN PROGRESS.

Manager review: PENDING.

Architectural decision:

- Upgrade Fastify to 5.12.3.
- Remove numeric TRUST_PROXY from the application contract.
- Prefer explicit proxy address/CIDR trust.
- Document controlled TRUST_PROXY=true only for an unreachable origin behind a sanitizing proxy.
- Derive ALB trusted proxy CIDRs from the public subnet resources.
- No AWS apply performed.

Batch 3 implementation: NOT STARTED.

## Planned next batches

- Batch 1 — controlled fast-uri security maintenance
- Batch 2 — Fastify security/architecture compatibility challenge
- Batch 3 — bounded cross-module product change

Current gate: Batch 2 implementation in progress / Manager review pending.
