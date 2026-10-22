# Node projects

## Package manager

Use only the manager the project uses, and keep its lockfile:

| Lockfile | Manager |
|---|---|
| `npm-shrinkwrap.json` (wins over `package-lock.json`) or `package-lock.json` | npm |
| `pnpm-lock.yaml` | pnpm |
| `yarn.lock` | yarn |
| `bun.lock` / `bun.lockb` | bun |

- A `packageManager` field in `package.json` takes precedence. Its manager version is a toolchain
  pin: report a newer release, change it only if the user asks.
- Lockfiles of two managers, or a `packageManager` field that disagrees with the lockfile: stop
  and ask which manager the project uses.
- No lockfile: use the `packageManager` field, else npm. Report it, and list a lockfile the
  install creates as a new file.
- Never delete the lockfile to force a fresh resolution.

## Inventory

For every `package.json` (all workspaces), record:

- `dependencies`, `devDependencies` and `optionalDependencies`: name and current specifier,
  classified by source:
  - Registry versions (`^1.2.3`, `1.2.3`, `~1.2`, `>=1`): updated by this skill.
  - Aliases (`npm:other-pkg@^1.0.0`): update the version after the `@`, keep the alias.
  - Everything else (`workspace:`, `file:`, `link:`, `portal:`, `catalog:`, git or URL
    specifiers): not a registry version. Leave it unchanged and report it.
- `peerDependencies`, `overrides` / `resolutions`, `engines`, `packageManager`.
- The Node runtime the project runs on: `.nvmrc`, `.node-version`, `engines.node`, `node:`
  images in Dockerfiles and CI.
- Registry configuration (`.npmrc`, `.yarnrc.yml`). It applies per directory, so run every
  registry and manager command from the directory of the `package.json` concerned; from
  elsewhere a scoped private package can falsely resolve as "not found". Private registries need
  their auth variables set. If a private package cannot be resolved, report it; never skip it
  silently.

## Baseline (before any change)

Run the audit (§ Vulnerability audit) and record the "before" findings.

## Resolve latest

- The registry is the source of truth. Query it with the project's own manager, so its registry
  and auth configuration apply (`npm view`, `pnpm view`, `yarn npm info` or the equivalent),
  bypassing cached metadata (npm and pnpm: `--prefer-online`). `npm outdated` and its
  equivalents can serve stale data and miss releases: use them only as a starting list, and
  query every package, including those they show as current.
- Target: the highest stable version in the package's version list, by semantic version order,
  skipping versions marked deprecated. Dist-tags such as `latest` are set by the publisher and
  can point elsewhere: if `latest` differs from the target, report it. If the current version
  is a pre-release on a newer line than every stable release (e.g. `5.0.0-beta.30` against
  stable `4.24.15`), keep the current pin and report it (critical rule 3).

## Update

- Target: every registry entry in `dependencies`, `devDependencies` and `optionalDependencies` at
  its latest stable version, majors included.
- Pin style: caret on the exact latest version, `^x.y.z`. Replace exact pins and wider ranges
  with that form, unless the project's instructions or docs set another pinning rule; then
  follow the project.
- Before each update, check the target's `engines` and peer requirements against the project's
  Node runtime and the other packages, patches and minors included.
- `peerDependencies` are the package's public contract for its consumers. Widen a peer range
  only when an update requires it, and report it. After migrating code to a new API, check that
  it still works with the lowest version each peer range allows; if it does not, the range would
  narrow: ask the user first, and never narrow one without approval.
- `overrides` / `resolutions`: re-check each after the update; report any that is no longer
  needed or now pins a vulnerable version. Do not drop one without evidence.
- Install with the project's manager (`npm install` etc.) so the lockfile is rewritten
  consistently.
- Peer conflicts: resolve them by aligning versions (often a pinned dev or test tool on an old
  major). Never bypass them with `--force` or `--legacy-peer-deps`.
- The Node runtime and container images stay as they are unless the user asks; report any skew
  (e.g. `@types/node` ahead of the runtime, after confirming the code uses no API the runtime
  lacks), runtime versions that differ between the container, CI and local environments, and
  floating image tags.
- Files a framework generates or rewrites on install, build or dev start (e.g. type declaration
  stubs) are kept exactly as the new version writes them, and listed in the report.

## Vulnerability audit [REQUIRED]

- Run the project's audit gate (an `audit` step in its scripts, CI or validate script) at its
  configured level, and the full audit as well. With no gate, the full audit is the gate.
- The gate must end with 0 findings. A transitive finding fixable within the allowed ranges is
  fixed with a lockfile-only update (`npm update <pkg>` or equivalent). A finding with no fixed
  release is reported with its advisory ID, not hidden. Findings outside the gate (e.g.
  dev-only) are fixed the same way when a lockfile-only update within allowed ranges fixes
  them, never with `audit fix --force`; the rest are reported.

## Deeper unused check (only when the user accepts it)

A direct dependency is really unused only when all six hold. Report each with its evidence:

1. No `import`, `require` or dynamic import of it anywhere in the source.
2. No mention in any config file (bundler, compiler, linter, test, CSS or framework config).
3. No use of its command-line binary in `package.json` scripts, Makefiles, CI or hook files.
4. No installed package declares it as a required peer dependency.
5. It is not a type package (`@types/x`) for a package that is still used.
6. No framework or tool the project uses loads it implicitly when installed (optional peers in
   `peerDependenciesMeta`, plugins discovered by name, e.g. an image processor a web framework
   picks up). Check that framework's documentation.

## Verify (in addition to the project's checks)

- Every script in `package.json` that checks or builds: lint, format check, type-check, tests,
  build (every build variant the project or its CI uses).
- The type-check covers the project's source files: a run that checked no files is not a pass.
- Web apps: start the dev server and the production build, request the main routes and any
  health endpoint, expect success, then stop the servers. Other runnable apps (CLI tools,
  workers): run the project's documented start or smoke checks.
- Generated committed files: if a script generates files that are committed, rerun it. Produce
  the "before" output from the original lockfile in a scratch copy. The output must be
  identical before and after, or every difference must be traced to a documented change in an
  updated package and listed in the report. An unexplained difference is a regression. If the
  committed files were already stale before the update, say so and leave them as committed.
