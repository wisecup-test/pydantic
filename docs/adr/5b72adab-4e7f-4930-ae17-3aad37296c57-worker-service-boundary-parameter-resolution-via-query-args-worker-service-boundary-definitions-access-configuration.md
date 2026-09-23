# Worker Service Boundary Parameter Resolution via query_args: Worker Service Boundary Definitions Access Configuration

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- The worker service boundary requires dynamic parameterization to identify target artifacts across external network calls.
- The implementation extracts parameter values using query_args.get to determine configuration state such as artifact versions.
- External assets are fetched asynchronously over the network, with anomalous responses and failures logged through console.error.
- The observed pattern is confined to a single worker script utilizing standard runtime capabilities rather than a structured multi-module boundary contract.

## Problem Statement

The worker runtime must ingest dynamic configuration parameters from incoming query inputs to coordinate external network asset retrieval, but relying on unvalidated query parameter lookups across service boundaries introduces coupling to query structure and lacks multi-module enforcement.

## Decision

1. MUST: Worker service boundary definitions MUST access configuration parameters through query_args lookups when resolving target resource versions before initiating remote network requests.

## Policy Block

- MUST Worker service boundary definitions MUST access configuration parameters through query_args lookups when resolving target resource versions before initiating remote network requests.

In scope:
- Worker execution boundaries that parse input query parameters to coordinate external resource retrieval.
- Runtime communication endpoints dispatching remote network fetch calls based on parsed version identifiers.

Out of scope:
- Internal modules communicating through synchronous in-memory functional interfaces without network boundaries.
- Static build routines where asset versions are established at compilation time rather than resolved dynamically via runtime query parameters.

## Rationale

- Extracting parameters directly via query_args provides a lightweight mechanism to identify runtime dependencies without introducing third-party client routing libraries.
- Network dispatch via fetch decoupled from static bundling allows on-demand retrieval of versioned resources across external client boundaries.
- Logging unexpected response formats and runtime exceptions via console.error establishes basic observability at the worker service perimeter.

## Consequences

Positive:
- Enables dynamic version selection at runtime through simple query parameter bindings.
- Eliminates bundling overhead by retrieving version-specific artifacts on demand across network boundaries.
- Provides immediate perimeter visibility for failed external client requests via standardized error logging.

Negative:
- Confines service definition logic to a single script without shared validation schemas across the broader codebase.
- Introduces runtime failure exposure if query parameters are missing, malformed, or point to unavailable remote resources.
- Relies on primitive runtime error output rather than structured telemetry or fallback handling.

## Alternatives

- Static pre-bundling of all target artifact versions directly into the worker distribution (rejected)
  Rejected because: Pre-bundling all versions substantially increases distribution payload size and prevents dynamic version switching
  When valid: When operating in isolated environments without network connectivity
- Structured schema-validated service definitions using an RPC or contract library (deferred)
  Rejected because: The current worker scope is limited to a single preview utility file where full schema validation overhead is not yet justified
  When valid: When multiple worker endpoints or bi-directional service contracts must be coordinated across services

## Risks

- External fetch failure when the specified version parameter does not resolve to an accessible remote artifact
  Mitigation: Validate query parameters and verify HTTP response status codes prior to executing downstream tasks
  Owner: Engineering Team
- Silent behavioral drift from unvalidated query parameter inputs
  Mitigation: Implement strict parameter checking and default fallback logic when query_args lookups return undefined
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Inspect worker entry scripts to locate query parameter parsing logic and ensure consistent key naming for version properties.
- Confirm network fetch handlers check response ok status and log unexpected responses using standard error output.

## Continuation Context


Verify commands:
- Discover the repository test runner from the root configuration and execute worker integration test suites.
- Discover and run the project code linter and type checker across worker scripts to verify query parameter handling and error branch coverage.

Accept when:
- Worker execution entry points successfully parse input query parameters to resolve external resource endpoints.
- Anomalous network responses and parsing exceptions trigger documented error logging routines without crashing the worker context.

## Enforcement

- Verified by: Continuous integration test suites and static analysis checks.
- Verified by: Code review verification on changes modifying worker request handlers or service boundaries.
- Violation handling: Pull requests bypassing parameter validation or omitting error handling at network boundaries will be blocked during review.
- Violation handling: Build failures triggered by linting or integration test checks must be resolved prior to merging.
- Exception process: Exceptions require documented architectural approval describing alternative parameter resolution or offline asset management strategies.