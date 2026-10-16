# Cursor pstack 0.15.25 sync report

Resolved live Git remotes on 2026-10-16, rather than trusting the saved checkout.

| Ref | Verified revision |
| --- | --- |
| Fork `origin/main` before update | `5c050f1` |
| Previous Cursor sync | `9f451cf875ad1239912762f67741e8e5ba6ac0f1` / 0.15.15 |
| Cursor `main` fetched tip | `f713f14203cd6420cec13cd7c55c32d598dbab68` / pstack 0.15.25 |
| Latest commit touching pstack at that tip | `99743e9c151c8537cbbf1f98bb38f34ef3368453` |
| Candidate branch | `sync/cursor-pstack-0.15.25` |
| Open Pstack candidate version | 1.7.0, not a tagged release |

Provenance is established by README, UPSTREAM.md, NOTICE.md, Git history, and the `cursor` remote: [authoritative Cursor pstack](https://github.com/cursor/plugins/tree/f713f14203cd6420cec13cd7c55c32d598dbab68/pstack). The other `upstream` remote points at the compatibility port and is not the content authority. Origin advertised only main when fetched; no newer remote adaptation branch was ignored.

## Changes and conflict resolution

Thirty pstack commits follow the recorded base. The repository audit/merge tools classified 26 verbatim imports, 20 clean merges, 43 review rows (including an existing test-path collision), and three unmapped distribution/documentation paths. Review rows are not all semantic conflicts. Review covered three-way content and the final diff.

Imported: ketchup, in-your-own-words, sharpen, shorter reasoning-led principles and review prompts, actual-app readiness, parent-as-runner semantics, fenced-plan parsing, complete review-thread pagination, default/explicit worktree base auditing, and a GitHub/Origin frontier instead of Graphite metadata.

Preserved: one shared skill tree; Claude/Codex provider boundaries; configured model/effort selections; inherit-parent for MCP-sensitive roles; no nearest-model fallback or implicit timeout; config homes; native preload; exact-head/fork-safe landing and operator gates. Overlapping prompt cuts keep these port contracts rather than restoring Cursor primitives. Shipping and autopilot retain their detailed safety steps while adding ketchup catch-ups. Cursor plugin manifests and hosted guide remain excluded. The existing exported plan validator and test suite are retained, with the upstream fence algorithm and three regression cases. The exact-head verifier registry includes the new skills.

## Validation

- Bun 1.4.0 frozen dependency installation passed.
- Plugin tools: **240 tests passed, 0 failed** across 11 files, including upstream pagination/frontier tests and the adapted fence regressions.
- Strict typecheck passed for watch-pr, runner, and plan checker. Orchestration files also receive an additional strict check.
- Static invariants and verifier development suite: **107 tests passed, 0 failed**, plus strict verifier typecheck. Run with `USER`/`LOGNAME` set to the actual `id -un` and `env -u CODEX_HOME` only for this fake-driver suite: its fixtures set HOME and otherwise inherit the managed read-only Codex home. No real login or config is changed.
- JSON manifests parse; versions and asset references match; `git diff --check` passes. Native Claude plugin validation cannot run here because Claude CLI is absent.
- Live doctor is **blocked**, with `Live gate requires an operator Mac (Darwin)` on Linux. No installed live proof, `live-gate` success, merge, release, or deployment is claimed. A verifier-changing candidate requires reviewed candidate code as publisher and candidate revision; follow `.claude/skills/verify-open-pstack/SKILL.md` on the supported environment.
- Diagrams are original HTML/inline SVG, with PNG previews and a printable PDF. Browser checks cover a 1440px viewport, 390px viewport (local SVG horizontal scrolling, no page overflow), and print media. All three figures have eight nodes and eight edges, within the selected skill's budgets.

Issues are disabled on this fork (GitHub returned HTTP 410). No settings were changed and no issue was opened in an unrelated repository; the PR is the durable work record.

## Reproduce

Run the CI commands in `.github/workflows/ci.yml`. For this environment's verifier fixture suite only, use `env -u CODEX_HOME USER="$(id -un)" LOGNAME="$(id -un)" PSTACK_STATIC_ONLY=1 bash tests/skill-collision-repro.sh` with Bun on PATH. Real live verification must retain the operator's existing native login setup and is a separate gate.

See [architecture analysis](pstack-architecture.md) and [Arabic diagram atlas](diagrams/pstack-workflow.html).
