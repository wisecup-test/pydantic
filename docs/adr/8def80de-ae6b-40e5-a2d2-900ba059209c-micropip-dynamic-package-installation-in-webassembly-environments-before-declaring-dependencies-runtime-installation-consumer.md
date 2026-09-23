# micropip Dynamic Package Installation in WebAssembly Environments: Before Declaring Dependencies Runtime Installation Consumer

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Context

- Execution of test suites within WebAssembly runtime boundaries requires loading compiled binary artifacts and their downstream dependencies directly into the interpreter environment.
- Standard system-level process spawning and native packaging mechanisms are constrained or unavailable in browser and sandboxed WebAssembly execution contexts.
- Dynamic in-memory installation via dedicated runtime loader APIs enables test harnesses to dynamically mount wheels and required validation packages into memory.

## Problem Statement

WebAssembly test harnesses cannot utilize standard operating system process management and host package installers to provision runtime environments. Without an in-runtime dependency loading strategy, verifying compiled binaries in sandboxed environments would require pre-building static monolithic runtime images for every dependency combination.

## Decision

1. MUST: Before declaring dependencies for in-runtime installation, the consumer MUST inspect the repository lock artifact to determine exact resolved versions.

## Policy Block

- MUST Before declaring dependencies for in-runtime installation, the consumer MUST inspect the repository lock artifact to determine exact resolved versions.

In scope:
- Test harness runners and validation scripts operating within WebAssembly runtime boundaries.
- In-browser or sandboxed validation suites that execute compiled wheels directly.

Out of scope:
- Native platform unit test and integration test runners executing directly on host operating systems.
- Standard build and packaging pipelines executing on native continuous integration runners.

Exceptions:
- EXC-17-001: Pre-built monolithic runtime images containing all dependencies are supplied directly by the execution platform

## Rationale

- Dynamic dependency loading through runtime installation APIs allows running standard test packages directly inside sandboxed environments without requiring specialized native host shims.
- Programmatic dependency installation inside the interpreter keeps test harness configuration closely coupled with execution logic while respecting sandbox execution constraints.
- Configuring interpreter limits explicitly prevents early runtime exhaustion in resource-constrained virtual environments.

## Consequences

Positive:
- Enables automated verification of compiled artifacts in WebAssembly environments matching browser and edge deployment models.
- Eliminates the need for external host-level package distribution machinery during sandboxed test execution.
- Maintains explicit programmatic control over the dependency matrix loaded into the test interpreter.

Negative:
- Incurs network and runtime overhead during harness initialization due to in-memory wheel downloading and extraction.
- Confines execution to single-threaded runtime semantics, limiting concurrency testing models.

## Alternatives

- Pre-bundling all test dependencies into a monolithic static runtime archive (rejected)
  Rejected because: Static bundling bloats binary distribution artifacts and complicates matrix testing across varying dependency combinations.
  When valid: When target environments run completely offline with immutable dependency graphs.
- Mocking runtime boundaries and testing only on host platforms (rejected)
  Rejected because: Fails to catch WebAssembly-specific compilation issues, memory boundary bugs, or missing platform symbols.
  When valid: For standard unit tests with no native compilation or architectural boundary constraints.

## Risks

- Network failures or repository unavailability during in-runtime dependency downloads causing harness flakiness
  Mitigation: Implement caching mechanisms for resolved wheel archives within the execution environment
  Owner: Core Engineering Team
- Interpreter memory exhaustion or recursion limit exceedance under complex test execution graphs
  Mitigation: Explicitly configure and tune runtime limits before invoking test suite entry points
  Owner: Core Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Locate the runtime test execution scripts in the repository and observe how the execution environment is bootstrapped.
- Ensure the compiled artifact wheel path is dynamically passed to the in-runtime installer prior to running test suites.

## Continuation Context


Verify commands:
- Discover the project's WebAssembly test runner script and execute it within the configured runtime container.
- Inspect the test execution logs to verify that all in-runtime dependency installations complete successfully before test execution starts.

Accept when:
- The runtime test runner completes with exit code zero and all test assertions pass in the sandboxed environment.
- Dynamic dependency resolution finishes without unhandled network or extraction exceptions.

## Enforcement

- Verified by: Automated continuous integration checks executing the WebAssembly verification suite on pull requests.
- Verified by: Peer code review of changes to harness initialization and runtime configuration routines.
- Violation handling: Pull requests failing the WebAssembly runtime test harness are blocked from merging.
- Violation handling: Unauthorized modifications to runtime boundary settings will require review and revision.
- Exception process: Exceptions must be documented via architectural review requests detailing why static pre-bundling or alternative test isolation is required.