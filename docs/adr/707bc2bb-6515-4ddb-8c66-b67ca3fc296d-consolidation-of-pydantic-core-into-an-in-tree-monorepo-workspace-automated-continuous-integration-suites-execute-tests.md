# Consolidation of pydantic-core into an in-tree monorepo workspace: Automated Continuous Integration Suites Execute Tests

Status: proposed
Date: 2026-10-01
Deciders: AI (signal conversion)

## Context

- Previously, pydantic-core was developed and maintained in a separate repository, requiring Pydantic to consume it as an external package pinned through pyproject.toml and uv.lock. Any cross-cutting enhancements or bug fixes spanning both the Python layer and the Rust validation core required disjoint PR workflows, multi-repository version bumps, and delayed integration testing.
- The repository has now incorporated pydantic-core directly into its source tree as a unified workspace. Workspace resolution is configured to run against in-tree sources, with unified CI matrices, joint coverage evaluation, and combined benchmark and documentation workflows.

## Problem Statement

Managing pydantic-core as an external dependency creates release latency, synchronization friction, and duplicated maintenance overhead between Pydantic's Python API and Rust validation engine.

## Decision

1. MUST: Automated continuous integration suites MUST execute tests, benchmarks, and coverage checks jointly across Pydantic and in-tree pydantic-core components.

## Policy Block

- MUST Automated continuous integration suites MUST execute tests, benchmarks, and coverage checks jointly across Pydantic and in-tree pydantic-core components.

In scope:
- Repository workspace configuration files (pyproject.toml, uv.lock, Makefile).
- Continuous integration and documentation workflows (.github/workflows/*, build-docs.sh).
- Core testing and benchmark configurations (tests/pydantic_core, codspeed).

Out of scope:
- External downstream third-party consumers installing Pydantic via pre-built wheels on package indices.

Exceptions:
- ex-1: Testing specific backward-compatibility matrix scenarios against published legacy releases.

## Rationale

- Eliminates cross-repository release latency and synchronization friction between Pydantic's Python API and the underlying Rust core.
- Unifies the test matrix, benchmark reporting (CodSpeed), coverage analysis, and automated dependency management (Dependabot) into a single operational cycle.
- Allows atomic pull requests that span both Python API definitions and Rust validation logic simultaneously.

## Consequences

Positive:
- Immediate feedback during local development when changing Rust validation internals alongside Python constructs.
- Synchronized documentation builds and benchmark comparisons against current core sources.
- Single repository issue tracking, PR reviews, and unified CI execution.

Negative:
- Increases monorepo complexity and requires Rust toolchains for developers building or testing full workspace changes.
- Introduces release management questions regarding standalone PyPI distribution of pydantic-core.

## Alternatives

- Developing and consuming pydantic-core as an external repository dependency bumped across version bumps in pyproject.toml and uv.lock (rejected)
  Rejected because: Creates cross-repository release latency, synchronization friction between the Python API and Rust validation core, and requires fragmented multi-stage testing.
- Maintaining standalone releases of pydantic-core on PyPI while housing sources in-tree (deferred)
  When valid: Valid if external downstream consumers demonstrate a strong need for standalone pydantic-core releases decoupled from Pydantic releases.

## Risks

- Increased CI build times and local setup complexity due to compiling in-tree Rust code.
  Mitigation: Optimize workspace build caching and uv integration across local and CI environments.
  Owner: Core Maintainers
- Breaking downstream consumers relying on independent, decoupled releases of pydantic-core.
  Mitigation: Determine standalone packaging and PyPI release coupling policies prior to final release tagging.
  Owner: Release Engineering

## Implementation Notes

- Configure uv workspace members in pyproject.toml to link the in-tree core directory.
- Update build-docs.sh and Makefile targets to point directly to in-tree core sources during local iteration and docs builds.
- Consolidate dependabot.yml to manage dependencies across both workspace parts under unified schedules.

## Continuation Context


Verify commands:
- Discover and execute workspace dependency resolution commands to confirm in-tree source binding for all workspace members.
- Discover and run the joint test suite, documentation build, and benchmark execution scripts across workspace components.

Accept when:
- Workspace tooling resolves pydantic-core locally without network fetch from external package registries.
- CI pipelines pass all validation, documentation, benchmark, and coverage steps against in-tree sources.

## Enforcement

- Verified by: CI workflow configurations verifying that workspace resolution loads local pydantic-core sources.
- Verified by: Linting and build check passes across pyproject.toml and uv.lock ensuring in-tree workspace membership.
- Verified by: Pull request reviews verifying joint test execution and workspace compliance.
- Violation handling: CI job failure if external pydantic-core packages are pulled instead of in-tree sources.
- Violation handling: PR rejection when PRs attempt to pin external pydantic-core releases for internal development runs.
- Exception process: Submit an architectural review request to Core Maintainers specifying rationale for external packaging overrides.

## References

- file:.github/dependabot.yml
- file:.github/workflows/ci.yml
- file:.github/workflows/codspeed.yml
- file:.github/workflows/docs-update.yml
- file:Makefile
- file:build-docs.sh
- file:docs/contributing.md
- file:pyproject.toml
- file:tests/pydantic_core
- file:uv.lock
- commit:41f6776e61ebafe01f48b2b4296ff6aa5cc62543
- commit:4630328b4526679ba3eab32fdc4b7c4491581f11
- commit:6800281ba87625346daf5826563740ded8a9851b
- commit:e35700cba1cf9bacd07a4fcafb2ca36ed8afe805
- commit:11c5a6bfaccbbf277af240f2904717b2a1d84f4d
- commit:8d2bbf1d5d21b3204a5f84f8eeea3709158b8340
- pr:#12486
- pr:#12490
- pr:#12491
- pr:#12496
- pr:#12504
- pr:#12554