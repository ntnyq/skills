---
name: release-engineering
description: Design, review, harden, or execute release workflows for npm packages and multi-platform artifacts. Use for release readiness, version/tag/dist-tag validation, GitHub Actions publishing, provenance, package packing, registry smoke tests, prereleases, stable promotion, bounded retries, or Chinese prompts such as "发布流程", "release 检查", "npm 发布", "预发布", or "发布验证". Do not use for ordinary CI-only changes or application deployment.
---

# Release Engineering

Build release workflows that verify the exact consumer artifact before any
irreversible publication step.

**Core constraint:** Never publish packages, create or move tags, create a
release, or promote a channel without explicit authorization for the exact
version, registry, channel, and repository.

## Scope

- Apply this skill to package and artifact readiness, publication, prereleases,
  stable promotion, and post-publish verification.
- Keep ordinary CI maintenance and application deployment outside this skill
  unless they are part of the requested package release path.
- Use the relevant package-development skill for source, API, and build design;
  this skill owns the release gates and external mutations.

## Workflow

- [ ] Inspect package manifests, workspace metadata, release scripts, workflows,
      current branch and tags, and existing release documentation.
- [ ] Identify the release mode and exact target: dry run, prerelease, stable
      release, promotion, or recovery; record version, tag, dist-tag, registry, and
      expected artifacts.
- [ ] Run read-only readiness checks, build and pack the artifacts, and verify
      their public entries, metadata, types, binaries, and platform packages.
- [ ] Install and smoke-test the packed or published artifacts in an isolated
      consumer project; use only bounded retries for known registry propagation.
- [ ] For review or dry-run tasks, report readiness without external mutations.
      Only when live execution was requested, obtain authorization immediately
      before the mutation, execute the approved target once, and verify its state.

## Readiness Gates

- Keep package versions, workspace manifests, release tags, changelogs, and
  release channels consistent.
- Run the repository's release-check equivalent before generating release notes
  or publishing.
- Inspect the packed file list and verify that every public export resolves from
  the artifact rather than from the source tree.
- Test required peer dependencies, optional dependencies, CLIs, native bindings,
  WASM files, and sidecar packages according to the package contract.
- Pin third-party GitHub Actions to full commit SHAs with readable version
  comments. Keep job permissions minimal and use provenance or trusted
  publishing when the repository supports it.

## Registry And Promotion Checks

- Install the exact version from the intended registry and dist-tag in a clean
  temporary project. Exercise representative public imports and commands.
- Retry only errors known to come from registry eventual consistency. Bound the
  attempts and delay; do not retry authentication, version-conflict, manifest,
  integrity, or functional-test failures.
- Promote a prerelease only when the candidate and target versions match, the
  candidate artifacts and commit are identified, required evidence has passed,
  and runtime changes are absent or explicitly approved.
- Keep machine-readable readiness evidence when promotion depends on multiple
  platforms, compatibility reports, or issue-severity gates.

## Failure Handling

- Stop after the first unexplained failure. Do not broaden retries, republish the
  same immutable version, or rewrite tags to force completion.
- Report which external mutations succeeded, which target failed, and the safest
  recovery options.
- Require new explicit authorization before deleting releases, moving tags,
  deprecating versions, changing dist-tags, or publishing a replacement version.

## Anti-Patterns

- Treating a successful source build as proof that the packed package works.
- Publishing before release checks, artifact inspection, or consumer smoke tests.
- Using floating action tags, broad workflow permissions, or unbounded retries.
- Hiding a partially completed release behind a generic failure message.

## Delivery Check

- Confirm the reported version, tag, dist-tag, registry, commit, and artifact set.
- Record the readiness commands and consumer smoke tests that passed.
- Verify the published metadata and provenance, or clearly describe the partial
  external state and the next authorized recovery step.
