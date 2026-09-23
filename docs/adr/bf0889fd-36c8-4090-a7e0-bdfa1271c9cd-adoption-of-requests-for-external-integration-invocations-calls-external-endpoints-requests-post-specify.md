# Adoption of Requests for External Integration Invocations: Calls External Endpoints Requests Post Specify

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Automation workflows require interaction with remote service GraphQL endpoints to query project metadata and participant activity.
- External network interactions introduce latency and potential transient failures across service boundaries.
- The codebase establishes synchronous HTTP boundary calls using an established client library to interface with external APIs.

## Problem Statement

Automated integration components that communicate with external GraphQL endpoints require a consistent mechanism for executing HTTP operations with predictable timeout handling, header authentication, and error reporting, avoiding unbounded requests and unhandled failure responses across service boundaries.

## Decision

1. MUST: Calls to external endpoints using requests.post MUST specify authorization headers and structured JSON request bodies.

## Policy Block

- MUST Calls to external endpoints using requests.post MUST specify authorization headers and structured JSON request bodies.

In scope:
- Automated integration routines and workflow scripts executing external network calls.
- Synchronous external API client invocations requiring structured JSON query payloads.

Out of scope:
- Internal in-memory unit tests that do not cross process or network boundaries.
- High-throughput asynchronous event processing services requiring non-blocking I/O architectures.

## Rationale

- Direct invocations of requests.post provide a straightforward synchronous interface for dispatching GraphQL queries with explicit timeouts.
- Explicit timeout constraints prevent workflow execution threads from hanging indefinitely during remote service degradation.
- Structured status checking and error logging ensure failures in external service responses are captured accurately for diagnostics.

## Consequences

Positive:
- Standardized HTTP request dispatching ensures consistent timeout enforcement across integration boundaries.
- Detailed status code validation and error logging facilitate rapid diagnosis of remote endpoint failures.
- Centralized client conventions reduce variance in how network credentials and payloads are structured.

Negative:
- Synchronous HTTP client invocations block workflow execution threads during external network round-trips.
- Direct dependency on remote endpoint availability introduces flakiness in integration runs when external services experience downtime.

## Alternatives

- Adopt an asynchronous HTTP client library for integration interactions (rejected)
  Rejected because: The sequential nature of the automated workflow script does not warrant the additional complexity of an asynchronous runtime event loop.
  When valid: When high concurrency or multiplexed streaming requests are required across integration boundaries.
- Execute shell subprocesses invoking command-line transfer utilities (rejected)
  Rejected because: Spawning external processes increases runtime overhead and complicates error handling compared to in-process library calls.
  When valid: When environments restrict language-level network client libraries but provide platform utilities.

## Risks

- External service rate limits or temporary downtime can cause integration workflows to fail unexpectedly.
  Mitigation: Implement structured error logging and evaluate retry policies with exponential backoff.
  Owner: engineering team
- Synchronous blocking calls can exhaust overall workflow time budgets if remote response times degrade.
  Mitigation: Enforce strict request timeout settings on all outbound client calls.
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure all outbound HTTP client calls configure a finite timeout parameter passed to the invocation method.
- Validate remote HTTP response status codes and log response payload contents when non-successful statuses are encountered.

## Continuation Context


Verify commands:
- Discover the test runner from the project repository configuration and execute the integration test suite.
- Discover the repository static analysis suite and verify that network client calls configure timeout parameters.

Accept when:
- Integration suites execute outbound network operations using the standard client with timeout parameters configured.
- Verification processes confirm all external API interaction handlers evaluate status codes and log diagnostic outputs on error.

## Enforcement

- Verified by: Automated test suites executing integration routines during continuous integration.
- Verified by: Peer code review verifying adherence to timeout configurations and response error handling.
- Violation handling: Integration pull requests containing unconfigured timeouts or unhandled client calls will be blocked from merging.
- Exception process: Exceptions must be documented in code review with technical justification approved by the repository maintainers.