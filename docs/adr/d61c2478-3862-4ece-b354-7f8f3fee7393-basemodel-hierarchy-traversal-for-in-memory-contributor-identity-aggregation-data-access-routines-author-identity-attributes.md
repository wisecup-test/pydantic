# BaseModel Hierarchy Traversal for In-Memory Contributor Identity Aggregation: Data Access Routines Author Identity Attributes

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- The repository includes automation routines that query external endpoints to collect participation metrics across issues, discussions, and pull requests.
- External query responses provide nested trees of comment nodes, discussion replies, and review entities containing participant identity metadata.
- Data access operations require isolating author logins and deduplicating participant identities across overlapping interaction contexts.

## Problem Statement

Automating contributor metric aggregation requires navigating deeply nested conversation hierarchies returned by external endpoints. Directly parsing raw response payloads with dynamic dictionary indexing leads to brittle data access code susceptible to schema drift, key errors, and unhandled null values when processing comments, reviews, and discussion replies.

## Decision

1. MUST: Data access routines MUST access author identity attributes exclusively through validated model properties rather than performing dynamic dictionary subscripting on raw payload responses.

## Policy Block

- MUST Data access routines MUST access author identity attributes exclusively through validated model properties rather than performing dynamic dictionary subscripting on raw payload responses.

In scope:
- Routines extracting participant identities from hierarchical external query payloads

Out of scope:
- Data pipelines operating on flat tabular records or direct database queries

## Rationale

- Defining explicit BaseModel schemas for response nodes provides structured contracts and static validation over external payload attributes.
- Accumulating logins into set structures directly through model properties guarantees unique participant tracking without manual deduplication passes.
- Isolating query execution and response normalization from metric aggregation prevents external schema modifications from cascading across downstream consumers.

## Consequences

Positive:
- Eliminates duplicate contributor identities automatically across nested conversation hierarchies.
- Enforces typed attribute access across comment and discussion structures, reducing runtime lookup exceptions.
- Decouples high-level identity collection from low-level payload parsing logic.

Negative:
- Requires defining and maintaining explicit schema classes for each nested level of response payloads.
- Increases memory consumption by materializing full schema instances prior to attribute extraction.

## Alternatives

- Direct dictionary traversal and key lookup on raw response payloads (rejected)
  Rejected because: Lacks structural validation and exposes consumers to lookup errors on unexpected response shapes or missing author fields
  When valid: Lightweight utility scripts requiring one-off ad-hoc data extraction without schema stability requirements
- Streaming event processing with immediate deduplication filters (rejected)
  Rejected because: Introduces unnecessary architectural complexity for small-to-medium batch extraction operations
  When valid: High-volume data pipelines where payload volume exceeds available system memory

## Risks

- External endpoint schema changes or field deprecations can break model validation during payload deserialization
  Mitigation: Implement defensive schema declarations with optional field defaults and validate integration responses against current external specifications
  Owner: engineering team
- Memory consumption scaling linearly with large comment histories when storing complete model graphs
  Mitigation: Extract required scalar identifiers during edge iteration and discard intermediate node instances
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
- Model classes representing comment and discussion nodes must declare author fields as optional to accommodate deleted users or system-generated messages.
- Deduplication sets should be instantiated at the start of traversal routines and populated using set addition operations during node iteration.

## Continuation Context


Verify commands:
- Discover and run the project static analysis validation script to verify that attribute access on BaseModel subclasses conforms to declared optionality.
- Identify and execute the repository test runner from the project configuration to validate data access and identity aggregation routines.

Accept when:
- All data aggregation routines successfully extract identity fields via validated BaseModel instances without runtime attribute errors.
- The project test suite passes with complete test coverage over hierarchical data model traversal logic.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test suites
- Verified by: Peer code review verifying model attribute navigation and null safety
- Violation handling: Pull requests introducing unvalidated dictionary access or direct payload indexing will be blocked during review
- Violation handling: Failed automated verification runs prevent merging into target branches
- Exception process: Submit an architectural exception request detailing the necessity of dynamic payload handling and obtain approval from the core engineering team.