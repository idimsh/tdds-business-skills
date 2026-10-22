# Go projects

## Inventory

Record each of these with its file and current value:

- Every `go.mod`: `require` entries (direct and indirect), `go` and `toolchain` directives, `tool`
  directives, `replace` directives.
- A `go.work` file, if present: its `use`, `replace`, `go` and `toolchain` lines. Run every Go
  command the way the project's CI does: in workspace mode, or per module with `GOWORK=off`.
- Vendoring: a `vendor/modules.txt` in a module or workspace.
- Tool pins outside `go.mod`: `go run module@version` and `go install module@version` in scripts,
  Makefiles and CI files; `tools.go` files behind a build tag; version variables in CI or scripts
  (linters, code generators, `govulncheck`).
- Go images: `golang:` and other base images in Dockerfiles, CI job images, compose files.

## Baseline (before any change)

- `go mod tidy -diff` in every module (Go 1.23+; with an older toolchain, run `go mod tidy` in a
  scratch copy and diff `go.mod`). A direct requirement it would drop appears unused
  (SKILL.md step 4a).
- The vulnerability scan for the "before" values: the project's own check if it has one
  (§ Vulnerability checker, step 2), otherwise the fallback scan (step 3) at the latest
  `govulncheck` release. Record the scanner version and findings.

## Resolve latest

- Same major: `go list -m -u all` in each module (run from the module directory). This also
  covers v0 → v1 and `+incompatible` versions, which keep their module path.
- Next major: the major version is part of the module path, as `/vN` (`example.com/mod/v2`) or,
  for `gopkg.in`, `.vN` (`gopkg.in/yaml.v3`). For each direct dependency, find newer major paths
  in the upstream repository (tags, README, package documentation), then confirm each with
  `go list -m <path>@latest`. Only a "not found" or "no matching versions" answer means the
  version does not exist; any other error (network, authentication, proxy) blocks that
  dependency (critical rule 2).
- Tools: `go list -m <tool-module>@latest`.
- Go itself: the latest stable release from `https://go.dev/dl/?mode=json`.
- Images: the registry's tag list; pick the exact tag matching the target Go version and the
  base variant the project already uses (e.g. same `-alpine`/`-bookworm` suffix).

`replace` directives pointing to local paths or forks are deliberate: keep them and report them
unless the project's docs say otherwise.

## Update

- `go get module@version` per dependency. For a new major, change the import paths in the code
  to the new module path and adapt the API.
- Update indirect dependencies too: `go get -u ./...` after the direct ones, then review the
  diff for unexpected major moves.
- Raise the `go` directive (and `toolchain`, if the project uses one) to the target Go version
  and keep Dockerfiles, CI images and any other Go pins on the same version.
- Tidy, in this order, in every module:
  1. Preview with `go mod tidy -diff` (it changes nothing). Tidy removes requirements nothing
     imports. Expected removals are old major paths replaced by their new path.
  2. Any other direct requirement in the preview needs the user's approval before tidy runs
     (SKILL.md step 4a). Without approval, skip `go mod tidy` for that module and report why.
  3. Run `go mod tidy`, then `go mod verify`.
- Vendoring: where `vendor/modules.txt` exists, regenerate it after tidy (`go mod vendor`, or
  `go work vendor` in a workspace), the way the project's docs or CI do.
- Pin style: exact versions and exact image tags. Where the project pins images by digest, keep
  digest pinning and resolve the digest of the new tag for the platforms the project builds.
  Never introduce floating tags (`latest`, `1`, `1.x`).

## Vulnerability checker [REQUIRED]

`govulncheck` (module `golang.org/x/vuln`) is the official Go vulnerability checker.

1. Find every place it is pinned (script, Makefile, CI, `tool` directive). Update each pin to
   the latest release from `go list -m golang.org/x/vuln@latest`.
2. Run the project's vulnerability check through its own script or target, so local and CI
   paths stay identical. Scanning must not modify `go.mod` or `go.sum`.
3. If the project has no vulnerability check, run
   `go run golang.org/x/vuln/cmd/govulncheck@<latest-version> ./...` in each module, report the
   result, and propose adding a pinned check. Do not add one unless asked.
4. Resolve every reachable finding by upgrading the affected module. A finding with no fixed
   release is reported, not hidden.

## Deeper unused check (only when the user accepts it)

A direct requirement is really unused only when all three hold. Report each with its evidence:

1. `go mod tidy -diff` removes it. Tidy considers every build tag and platform, but not files
   tagged `ignore` nor directories named `testdata` or starting with `.` or `_`.
2. No file tagged `ignore`, no file in those directories and no `tool` directive imports or
   names it.
3. No `go` command without an `@version` suffix (`run`, `install`, `build`, `test`, …) names one
   of its packages, in `//go:generate` lines, scripts, Makefiles or CI: such commands resolve
   through `go.mod`. Programs those commands build from the excluded directories count too.

## Verify (in addition to the project's checks)

- `go vet ./...` and `go test -race ./... -count=1` in every module.
- `go build` for every target the project ships: each `GOOS`/`GOARCH` in the Dockerfiles, CI or
  release config.
- Generated code: rerun the project's generators (`go generate`, API doc or mock generators).
  Produce the "before" output with the original `go.mod`/`go.sum` in a scratch copy. The output
  must be identical before and after, or every difference must be traced to a documented change
  in an updated package and listed in the report. An unexplained difference is a regression. If
  the committed files were already stale before the update, say so and leave them as committed.
- Contract or integration test suites affected by the updated packages, against local or
  isolated dependencies only.
