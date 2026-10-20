# Upstream synchronization

open-pstack tracks [Cursor's pstack](https://github.com/cursor/plugins/tree/main/pstack) while adapting Cursor-specific primitives for Claude Code and Codex.

## Current sync point

| Source | Value |
| --- | --- |
| Repository | `https://github.com/cursor/plugins.git` |
| Path | `pstack/` |
| Commit | `88d47514b82e7c44f00f2b2a72ff6dd12158038c` |
| Upstream version | `0.15.29` |
| open-pstack version | `1.8.0` |

The table above is the current Cursor sync point. The local Open Pstack 1.8.0 candidate imports this 0.15.29 sync; this does not identify a published release. `README-UPSTREAM.md` preserves the upstream pstack README verbatim. `CHANGES.md` and `NOTICE.md` describe the adaptations and provenance.

## 0.15.29 integration decisions

Imported the 16 upstream commits from `f713f14203cd6420cec13cd7c55c32d598dbab68` to `88d47514b82e7c44f00f2b2a72ff6dd12158038c` mechanically with `scripts/upstream-audit.py` and `scripts/upstream-merge.py`, then resolved six conflicts by semantic three-way review. Two new skills arrive whole: `create-formal-verification-skill` and `maintain-formal-verification-skill`. `setup-pstack` gains steps 10 and 11 offering the verification and formal-verification skills; `poteto-help` gains the matching routing line, the formal-verification state check, and the setup-offer sentence; `recipes.md` gains the `in-your-own-words`, `sharpen`, formal-verification, and `ketchup` entries. `ketchup` takes the upstream rewrite of its length rules verbatim, which makes it list every waiting item instead of cutting to a word count.

- `make-bot-ui` stays excluded: it is still built entirely from Cursor routine, webhook, and UI primitives that Claude Code and Codex do not share.
- The port keeps its `disable-model-invocation` exclusion for `typescript-best-practices`; upstream added the flag again for that skill, and poteto-mode calls it by name on Claude Code, so the flag would break that route. `ketchup` keeps the port's model-invocable frontmatter for the same reason, because autopilot calls it by name.
- `poteto-mode` keeps the port's Claude Code and Codex tool mappings over upstream's Cursor-only `AskQuestion`, `create-skill`, and `cursor-team-kit` references.
- `setup-pstack` keeps the port's provider-dispatch model sheet and rejects upstream's Cursor rules file; only the two new optional-offer steps are imported.
- The upstream rewording of `poteto-help`'s Cursor-guide and Custom Mode sentences is not imported, because the port already routes those decisions through the harness mapping files.

## 0.15.25 integration decisions

Resolved current remote refs on 2026-10-16. Previous port main was `5c050f1`, tracking Cursor `9f451cf875ad1239912762f67741e8e5ba6ac0f1` (0.15.15). Current Cursor main is `f713f14203cd6420cec13cd7c55c32d598dbab68`; its latest pstack-changing commit is `99743e9c151c8537cbbf1f98bb38f34ef3368453`. The recorded sync uses the resolved repository tip, whose pstack manifest is 0.15.25.

- Import the three new skills, prompt/principle refinements, parent-as-runner semantics, usable-app verification readiness, review-thread pagination, default-branch worktree auditing, and forge-derived orchestration frontier.
- Keep parent-owned provider dispatch, model defaults, explicit requested efforts, config homes, native skill preload, testable plan contract, and guarded fork/stack landing. Do not import Cursor's adaptive effort reductions, speed slugs, nearest-model fallback, runtime-based stall cancellation, cloud-only placement, or plugin discovery by highest version. A caller-supplied external deadline remains explicit.
- Keep the port's detailed shipping/autopilot safety steps where upstream shortened them. Add ketchup catch-ups without removing exact-head/base/lease guards. Preserve native tool mappings in overlapping prose cuts. `ketchup` stays model-invocable because autopilot calls it; `in-your-own-words` and `sharpen` are explicit user entry points.
- Adapt the fence parser inside the existing exported validator and preserve the port's broader test/typecheck scripts. Register new skills with the exact-head verifier. The new Cursor root `plugin.json`, Cursor manifest, and external guide are not port distribution files.
- Conflicts were resolved by semantic three-way review; no wholesale replacement of the adapted tree. The sync report records the scope and validation limits.

## Upstream-only exclusions

- The model-default changes in `df58112` remain port-specific: keep the Opus max / GPT-6.1 Sol max / Grok xhigh panel and Sol code roles, plus native-parent Why and Reflect roles. These keep the existing Claude Code/Codex provider routes and configured sheets usable. The upstream two-model Opus xhigh / Grok panel is not substituted silently.
- The new `benchmark-checklist` is model-invocable because poteto-mode and performance playbooks call it. The new principle uses `user-invocable: false`, as all port principle leaves do. `correct` and `poteto-help` retain explicit-invocation metadata.
- `poteto-help` uses the shared harness/config-home mappings and external links to Cursor guide pages. Cursor Custom Modes, UI shortcuts, `/add-plugin`, and excluded `make-bot-ui` are not advertised as port features. The Cursor guide remains an external reference rather than a duplicated tree.
- Hourly audits use `/loop 1h` in Claude Code and the supported Codex cadence mapping. The local plan contract keeps standing orders and installed-plugin reads; it does not restore Cursor `/goal`, Cursor source paths, or elapsed-time cancellation. Fork-safe merge and lease protections remain intact.
- `correct` follows the port's behavior and negative-path test contract instead of requiring deletion solely because a test uses a relational assertion.

- Commits `799151d` and `6fecddb` add and relocate `make-bot-ui`. It depends on Cursor routines, webhook events, and UI primitives that Claude Code and Codex do not share.
- Four `disable-model-invocation: true` lines from `73f8be4` are not applied to `how`, `why`, `unslop`, or `typescript-best-practices`. Poteto-mode invokes those skills by name, and the flag blocks that route on Claude Code.
- The default-model hunks for `bug-fix`, `perf-issue`, and `hillclimb` from `23a56e2`, `889ec4b`, and `70b2dc8` are not applied. Those frequent code-writing roles stay on `codex:gpt-6.1-sol@max`.
- `5bf2b15`'s setup budget question, its `# budget` line, and its step down to a lower detected effort are not applied. Setup already asks one requested effort per assigned family, and the step-down would silently lower a requested effort.
- `12d587d`'s rule that reruns a rejected configured entry on its family default or the closest valid slug is not applied. An unavailable model stays a named dropout per `provider-dispatch.md`.
- The expected-runtime column in `70b2dc8`'s `children.tsv` and its expected-runtime stuck test are not applied. A lane is stuck only on affirmative failure evidence.
- The explicit Grok, Opus, and Sol defaults for the Why and Reflect roles are not applied. Those roles stay on `inherit-parent` because the external runner omits the parent's MCP servers.
- The Claude manifest does not take the logo field from `efa2a53` because Claude Code has no schema for it. The shared asset is exposed through the Codex manifest instead.

## Local port-patch ledger

- **#120 — harness config homes** (tracked for #105): Cursor's setup destination is `~/.cursor/rules/`. The shared port resolves nonempty `CLAUDE_CONFIG_DIR` / `CODEX_HOME`, otherwise `$HOME/.claude` / `$HOME/.codex`, once at `plugins/pstack/skills/poteto-mode/references/codex-tools.md#harness-config-homes`. `setup-pstack/SKILL.md` reuses that home for reads, sheet/integration writes, snapshots, restoration, and readback; Claude preserves the literal legacy import at the default home and renders exactly `@./pstack-models.md` for redirected homes, keeping spaces and `#` in the config-directory name out of the import line; reruns replace the one import whose target basename is `pstack-models.md`, append if absent, and stop if duplicated. Preserve this intentional divergence during upstream syncs. The named config-home invariant in `tests/skill-collision-repro.sh` rejects literal default destinations outside the legacy import, requires the literal default-home rendering and zero/one/many import rules, and verifies unset/empty/space-containing resolution without daily writes. See `CHANGES.md` for the port correction and `tests/setup-config-home-repro.sh` for redirected live-evidence preparation. The Cursor sync point is unchanged.

## Check for changes

The repository already names Cursor's repository as the `cursor` remote in the maintainer checkout. A fresh clone can add it once:

```shell
git remote add cursor https://github.com/cursor/plugins.git
```

Fetch and inspect only commits that touched pstack after the recorded sync point:

```shell
git fetch cursor main
git log --oneline f713f14203cd6420cec13cd7c55c32d598dbab68..cursor/main -- pstack
git diff --stat f713f14203cd6420cec13cd7c55c32d598dbab68..cursor/main -- pstack
```

No output means the tracked pstack tree has not changed. This comparison does not need a polling service or generated mirror branch.

## Incorporate a change

1. Create or update a GitHub issue in this repository and branch from current remote `main`. If Issues are disabled, record the work in the PR without changing repository settings.
2. Read each upstream pstack commit in order. Bring over its intent and content, then apply only the Claude Code and Codex substitutions documented in `CHANGES.md`.
3. Keep one shared `plugins/pstack/skills/` tree. Put harness translation in the existing `codex-tools.md` and provider routing in `provider-dispatch.md`; do not fork a skill per harness.
4. Update the commit and version in this file, the affected provenance rows in `NOTICE.md`, and `README-UPSTREAM.md` when upstream changes it.
5. Run CI-equivalent checks locally, then run the installed Claude Code and Codex behavioral lanes required by the changed surface. Unit tests alone are not a release gate.
6. Merge the reviewed PR before tagging the next open-pstack release.

Cursor's version and open-pstack's version are independent. Cursor's version identifies the imported content; open-pstack's version identifies the cross-harness distribution.
