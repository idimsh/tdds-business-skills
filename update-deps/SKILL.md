---
name: update-deps
description: >-
  Update a project's dependencies to their latest stable releases, majors included, adapt the code
  to API changes, and prove it with the project's own checks and vulnerability scans. Supports Go
  (modules, Go toolchain, pinned tools and images, govulncheck) and Node (npm/pnpm/yarn/bun
  packages). Use when asked to update, upgrade or bump dependencies or packages, or to run
  update-deps. Say "dry run" for a read-only upgrade plan.
---

# Update dependencies

## Critical rules [REQUIRED — no exceptions]

1. **Follow the project's rules.** Discover them before any change: the agent instruction files
   (`AGENTS.md`, `CLAUDE.md` or equivalent), the docs they point to, contributing guides and CI
   configuration. Obey them for the whole run. Where they differ from this skill (pinning
   style, version holds, workflow, forbidden commands), the project wins.
2. **Evidence, not memory.** Versions move faster than training data. Resolve every target
   version from the registry, and read every changelog or migration guide from the official
   source, during this run. Never state a version, an API change or a check result you did not
   see. If a target cannot be verified (registry, authentication or network failure), keep that
   dependency at its current version, continue with the others, and report it as blocked.
3. **Stable releases only.** Never move to a pre-release. Keep an existing pre-release pin only
   while no newer stable release exists for it, and never treat a lower version as an upgrade.
4. **Adapt code, never weaken checks.** Fix the code to the new APIs. Do not disable lint
   rules, add suppression comments, loosen compiler or type settings, skip tests, or force
   dependency resolution (`--force`, `--legacy-peer-deps`, `replace` hacks) to get green.
5. **All the project's checks pass.** Discover every static check and test the project defines:
   CI jobs, validate scripts, package or Makefile targets, pre-commit hooks (lint, format,
   type-check, tests, build, vulnerability scan). Run all of them. The update is done only when
   every one passes.
6. **Version control per the project's rules.** Without such rules, do not commit, push or
   switch branches: list the changed files and propose a commit message.
7. **Report honestly.** A check that failed or could not run is reported as such, with its
   output. A dependency left behind is reported with the blocker and its evidence.

## Modes

- **Update** (default): steps 1–7.
- **Dry run** (user says "dry run", "plan only" or similar): steps 1–4 with read-only commands
  only (registry queries, `npm outdated`, `go list -m -u`, reading code and changelogs). Install
  nothing and change no project file, except files the project's rules require you to write.
  As a baseline, run checks that leave the project's files unchanged (audit, lint, format
  check, type-check without output). Before running one, read its full command chain,
  including scripts the package manager runs automatically before or after it (e.g. `pre`/
  `post` scripts); skip it if any part writes files or its effect is unclear. Record the
  version-control status before and after, and report any difference.
  Deliver the step 7 report as a plan: per dependency the target version, the APIs the code
  uses, the breaking changes found and the code changes you would make. Label every check
  "baseline run" (with its result), "planned", or "not run" (with the reason). Effects only a
  run can settle (e.g. new lint rules) are listed as "to confirm by running <check>".

## Workflow

1. **Load context.** Apply rule 1. Record the project's pinning convention, version holds and
   the full list of checks for rule 5, with the command that runs each.
2. **Detect project types.** Search the repository (root and subdirectories, excluding
   vendored and generated folders such as `node_modules`, `vendor`, `dist`, `.git`):
   - `go.mod` → Go. Load `references/go.md`.
   - `package.json` → Node. Load `references/node.md`.
   - Several manifests → handle each module or package; a repository can be both.
   - Anything else (Terraform, Python, Rust, …) → not covered. Report it and leave it unchanged.

   Announce what you found before continuing: "Detected: Go module `src/go.mod`; Node package
   `web/package.json` (npm)".
3. **Inventory and baseline.** Record every pinned version the type reference lists, with its
   file and the current value: the "before" column of the report. Before changing anything, run
   the vulnerability scans from the type reference and record each scanner's version and
   findings: the "before" vulnerability values.
4. **Resolve and research.**
   a. **Usage, for every direct dependency.** Find how this project uses it: imports, called
      APIs, config keys, CLI commands. A direct dependency with no use found is not updated.
      Report it as "appears unused" with what you searched, suggest removing it, and offer the
      deeper check from the type reference to confirm it is really unused. Run that check only
      when the user accepts, and never remove a dependency without the user's approval.
   b. **Resolve.** Find the latest stable version of each dependency and classify the change as
      patch, minor or major.
   c. **Research** every minor and major change, every change of a first-party package (one
      published by the project's own organisation), patches included, and every major change an
      update brings into a transitive package that the project's code or config uses directly
      (e.g. a bundler's engine through its plugin API). Read the official changelog, release
      notes and migration guide for every version between current and target, and record the
      source URL. If the official notes are missing or incomplete, compare the release's source
      or merged changes between the two versions, and record the gap.
   d. **Decide** what code must change, or record that the used APIs are unaffected.

   When the runtime supports sub-agents, research independent major updates in parallel, one
   package per sub-agent. Hand each one: the project rules that apply, the manifest's
   directory, the package with current and target versions, the type reference, and the
   instruction to research read-only. Each returns: used APIs with file paths, breaking changes
   that hit them, required code changes, source URLs. Check the returned sources before using
   the conclusions.
5. **Update.** Apply patch and minor updates as one batch, then verify. Apply major updates one
   at a time (packages released together, such as a framework and its type definitions or
   plugins, move together), adapt the code, then verify before the next one. Follow the type
   reference for exact commands and pin style.
6. **Verify.** Run every check from step 1 plus the extra checks in the type reference; all
   must pass (rule 5). Save long output to a temporary file and read the summary and failures
   instead of loading it whole. A failure caused by the update is fixed in code (rule 4) and
   re-verified. If the latest release cannot work, investigate first; then keep the newest
   release that works and report the blocker with evidence.
7. **Document and report.** Update the project docs that state dependency or tool versions,
   following the project's documentation rules. Then report:
   - **Detected:** project types and manifests.
   - **Versions:** table of dependency | before | after | change type (patch/minor/major).
   - **API changes:** per researched update, what changed, what code was adapted (files), and
     the source (URL or source comparison); or "used APIs unaffected".
   - **Verification:** each check with its command, exit status and a one-line output summary.
   - **Vulnerabilities:** scanner name and version, findings before and after.
   - **Held back or blocked:** each dependency not at latest and each vulnerability left open,
     with the reason and evidence, including dependencies that appear unused, with the offer of
     the deeper check.
   - **Changed files** and a **proposed commit message**.

## Final check before reporting

- [ ] The project's rules were discovered and obeyed; they overrode this skill where they differ.
- [ ] Every target version came from the registry in this run; no pre-release introduced.
- [ ] Every researched update has its used APIs checked against its official notes, or against
      a recorded source comparison where the notes are missing or incomplete.
- [ ] No check was weakened, skipped or forced; every project check passes (update mode).
- [ ] Every check is reported with what really happened in this run; dry-run checks are
      labelled "baseline run", "planned" or "not run".
- [ ] No dependency was removed without the user's approval.
- [ ] Version control was handled per the project's rules (default: no commits).
