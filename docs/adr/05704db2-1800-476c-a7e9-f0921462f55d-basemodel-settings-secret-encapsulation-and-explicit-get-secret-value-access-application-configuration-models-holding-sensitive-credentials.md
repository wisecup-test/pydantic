# BaseModel Settings Secret Encapsulation and Explicit get_secret_value Access: Application Configuration Models Holding Sensitive Credentials

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Automated workflows and script tasks require authentication credentials to interact with external service APIs and query endpoints.
- Diagnostic logging during execution serializes application configuration state, creating an operational risk of exposing sensitive authentication tokens in plain text.
- Application configuration relies on structured data schemas that inherit from BaseModel to parse, validate, and manage environment parameters.
- Credential management requires strict encapsulation within configuration structures such that secret tokens remain masked during serialization and are retrieved exclusively at external client invocation boundaries.

## Problem Statement

Exposing authentication credentials in application execution logs or passing unmasked secret strings across internal application boundaries presents a severe security risk. Scripts that log configuration state using serialization routines such as model_dump_json risk emitting sensitive access tokens unless secrets are encapsulated in specialized types and retrieved only at point-of-use via dedicated accessors.

## Decision

1. MUST: Application configuration models holding sensitive credentials MUST encapsulate secret values within specialized secret attributes on BaseModel configurations rather than plain string fields.

## Policy Block

- MUST Application configuration models holding sensitive credentials MUST encapsulate secret values within specialized secret attributes on BaseModel configurations rather than plain string fields.

In scope:
- Configuration schemas and settings definitions that ingest, manage, or transport authentication credentials.
- Outbound client dispatch boundaries where external HTTP and query requests require authentication headers.

Out of scope:
- Public, non-sensitive configuration values such as URLs, timeouts, and pagination cursors.
- Internal domain validation schemas that do not process secret credentials or external API tokens.

## Rationale

- Encapsulating credential values within dedicated secret wrappers on BaseModel settings ensures that configuration dumps via model_dump_json automatically redact token values in execution logs.
- Restricting token unwrapping to explicit get_secret_value calls creates an auditable boundary where sensitive data is exposed only at the exact point of external client request construction.
- Consolidating credential access patterns prevents ad-hoc environment variable reads and reduces the surface area for credential leakage.

## Consequences

Positive:
- Eliminates accidental credential exposure in application execution logs during routine diagnostic serialization.
- Provides a centralized and structured interface for managing sensitive configuration inputs.
- Enforces clear demarcation between serialized configuration state and active credential material.

Negative:
- Requires explicit unwrapping calls using get_secret_value whenever headers or authentication payloads are constructed.
- Increases boilerplate when passing configuration models across modules that do not directly execute client calls.

## Alternatives

- Direct environment variable access and standard string configuration fields (rejected)
  Rejected because: Storing secrets as standard string fields in BaseModel settings causes plain-text token values to be printed during model_dump_json serialization, creating significant log leakage risk.
  When valid: Valid only in trivial standalone scripts with no logging infrastructure and no serialized configuration inspection.
- Manual custom string redaction functions applied before logging configuration state (rejected)
  Rejected because: Manual redaction is error-prone, fragile to schema modifications, and easily omitted by developers during diagnostic logging.
  When valid: Valid in legacy environments lacking built-in secret field masking support in their data modeling framework.

## Risks

- Developers might inadvertently log the return value of get_secret_value directly in debug statements.
  Mitigation: Enforce static analysis rules and automated log scanning to detect credential string leakage.
  Owner: engineering team
- Failure to discover exact locked dependencies could lead to incompatible schema serialization behavior.
  Mitigation: Adhere to the mandatory lock-version grounding policy prior to modifying configuration schemas.
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
- Define all credential properties on BaseModel settings structures using specialized secret string types to ensure automatic redaction during serialization.
- Isolate calls to get_secret_value to the construction of external authorization headers immediately preceding external request execution.

## Continuation Context


Verify commands:
- Discover and run the project static analysis suite to verify that secrets are not exposed in plaintext string fields.
- Discover and run the repository test suite to confirm configuration serialization masks credentials under model_dump_json.

Accept when:
- Configuration models serialize without revealing plaintext secrets when processed through model_dump_json.
- Secret credentials are retrieved exclusively through get_secret_value at external client dispatch boundaries.

## Enforcement

- Verified by: Automated static analysis checks in continuous integration pipelines.
- Verified by: Peer code review auditing all configuration model definitions and logging statements.
- Violation handling: Code reviews must reject pull requests that expose credentials in plain strings or log get_secret_value outputs.
- Violation handling: Immediate remediation and token revocation if credentials appear unmasked in test or production logs.
- Exception process: Exceptions require documented security approval detailing compensating controls and justification.