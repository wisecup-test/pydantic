# Standard Library logging Adoption for Operational Script Diagnostics: Before Modifying Extending Logging Facilities Operational

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Operational automation scripts interface with external network APIs to retrieve and process repository activity data.
- Network requests and remote API operations are subject to non-200 status codes and payload-level error responses that require diagnostic recording.
- Standardized logging calls provide diagnostic traces in operational execution logs without introducing external telemetry dependencies.

## Problem Statement

Automated operational scripts require consistent failure reporting and execution configuration visibility when executing external network operations, without adding heavy telemetry infrastructure dependencies.

## Decision

1. MUST: Before modifying or extending logging facilities in operational scripts, the consumer MUST inspect the repository dependency configuration to resolve the environmental logging dependencies.

## Policy Block

- MUST Before modifying or extending logging facilities in operational scripts, the consumer MUST inspect the repository dependency configuration to resolve the environmental logging dependencies.

In scope:
- Operational scripts and automation tasks performing external HTTP requests and configuration loading.

Out of scope:
- Core library runtime modules and production services governed by dedicated structured telemetry frameworks.

## Rationale

- Directly utilizing logging.error and logging.info captures critical runtime failures and configuration snapshots into standard execution streams.
- Using the standard logging library avoids introducing external dependencies into lightweight operational automation scripts.
- Capturing raw error payloads alongside contextual iteration markers enables rapid root-cause analysis during script execution failures.

## Consequences

Positive:
- Improved diagnostic visibility into failed external client requests and response payloads.
- Zero external dependency footprint for basic script observability.
- Consistent recording of runtime configuration states prior to external interactions.

Negative:
- Standard logging output lacks structured JSON schema aggregation across distributed environments.
- Unbounded error text logging risks log bloat if external endpoints return large error payloads.

## Alternatives

- Direct standard output emission using print statements (rejected)
  Rejected because: Print statements lack severity levels, structured routing, and standard log stream configuration.
  When valid: Local throwaway scripts or minimal interactive command-line utilities.
- Adoption of an external structured logging framework (rejected)
  Rejected because: Third-party logging frameworks introduce external dependencies and configuration overhead unnecessary for isolated operational scripts.
  When valid: Production multi-service applications requiring distributed tracing and unified schema log streaming.

## Risks

- Sensitive secrets or tokens leaked into log streams when recording full response texts or configurations
  Mitigation: Enforce secret masking and sanitization before passing objects to logging functions.
  Owner: Engineering team
- Excessive log volume from printing unconstrained response payloads on repeated failures
  Mitigation: Truncate response text payloads or cap error reporting depth.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Initialize log formatting and severity levels at the entry point of operational scripts prior to executing external client calls.
- Extract and sanitize error messages from response structures before passing them to logging invocations.

## Continuation Context


Verify commands:
- Discover and run the repository static analysis suite to verify logging calls follow required conventions.
- Discover and execute test suites covering failure paths in external client interactions.

Accept when:
- Static analysis validates that error and lifecycle events invoke the standard logging interface without linting violations.
- Automated tests confirm error responses and configuration events are properly recorded in log outputs.

## Enforcement

- Verified by: Static analysis and linting checks in automated integration workflows.
- Verified by: Peer code review for changes involving script error handling and observability.
- Violation handling: Pull requests introducing unhandled failures or unmasked logging of sensitive information must be revised before approval.
- Exception process: Exceptions must be documented in pull request reviews with explicit approval from technical maintainers.