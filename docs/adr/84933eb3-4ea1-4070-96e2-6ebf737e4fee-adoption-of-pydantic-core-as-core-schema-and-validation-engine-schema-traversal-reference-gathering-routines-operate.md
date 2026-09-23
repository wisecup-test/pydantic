# Adoption of pydantic_core as Core Schema and Validation Engine: Schema Traversal Reference Gathering Routines Operate

Status: proposed
Date: 2025-02-17
Deciders: Detection Pipeline (automated)

## Context

- High-level model construction and schema generation require an efficient and consistent intermediate representation for data validation and serialization.
- Internal modules coordinate schema gathering, reference traversal, and metadata extraction to feed a unified low-level execution engine.
- Deferred model building and recursive schema references necessitate standardized handler callbacks and mock wrappers that conform to core engine protocols.

## Problem Statement

Model construction and validation logic require a unified, highly optimized intermediate representation to avoid redundant validation routines, maintain consistent error structures, and cleanly separate high-level Python class definitions from low-level execution mechanics.

## Decision

1. MUST: Schema traversal and reference gathering routines MUST operate on core schema definitions conforming to pydantic_core core_schema contracts.

## Policy Block

- MUST Schema traversal and reference gathering routines MUST operate on core schema definitions conforming to pydantic_core core_schema contracts.

In scope:
- Internal modules responsible for model construction, schema gathering, and schema generation.
- Components producing or transforming core schema representations for validation and serialization.
- Mocking structures managing deferred model initialization and validation rebuilding.

Out of scope:
- Public API surfaces that do not interact with core schema generation or compilation.
- Standalone documentation plugins and independent conversion utilities.

## Rationale

- Evidence across twelve internal modules demonstrates consistent reliance on pydantic_core core_schema contracts for schema traversal, reference resolution, and mock instantiation.
- Adopting pydantic_core establishes a clear separation of concerns where Python metaclasses inspect definitions while the core engine handles validation execution.
- Standardized core schema dictionary contracts allow modular composition of complex generic types, metadata, and discriminator definitions.

## Consequences

Positive:
- Standardizes intermediate representation for validation and serialization across all model types.
- Enables significant performance optimizations by delegating compute-heavy validation to an optimized core engine.
- Provides consistent error schemas and metadata resolution mechanisms across all object structures.

Negative:
- Introduces direct coupling between high-level Python class definitions and low-level core schema representations.
- Increases debugging complexity when inspecting generated core schema dictionaries and deferred reference graphs.
- Requires synchronization between Python-level error abstractions and core validation error structures.

## Alternatives

- Pure Python Validation and Serialization Engine (rejected)
  Rejected because: Executing data validation and recursive schema traversal entirely within pure Python runtime structures incurs unacceptable performance overhead for complex data hierarchies.
  When valid: Valid only in environments where native binary compilation or external C-extensions are strictly prohibited.
- Ad-hoc Per-Model Custom Validation Functions (rejected)
  Rejected because: Generating custom validator functions on the fly without unified core schema contracts creates inconsistent error reporting and prevents shared schema optimizations.
  When valid: Valid only in minimalist libraries with flat schema requirements and no nested model hierarchies.

## Risks

- Breaking changes in the core engine schema protocol could cause runtime failures across schema generation modules.
  Mitigation: Pin exact core library versions via lock artifacts and validate schema structures through automated integration suites.
  Owner: engineering team
- Circular references in complex models may cause infinite recursion during core schema traversal or reference resolution.
  Mitigation: Implement cycle detection and reference unrolling within schema traversal handlers before passing schemas to the core engine.
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
- Encapsulate schema gathering and reference cleanup in dedicated internal traversal utilities.
- Ensure deferred build models instantiate mock validators that fail gracefully until explicit rebuilding is triggered.
- Maintain strict separation between Python-facing metaclass mechanics and low-level schema dictionary construction.

## Continuation Context


Verify commands:
- Discover and execute the project verification script from the root repository configuration to run all core schema test suites.
- Discover and execute the static type analysis tool across internal schema generation modules to ensure compliance with core schema protocols.

Accept when:
- All schema generation, model construction, and reference resolution tests pass against the resolved validation engine.
- Static analysis confirms all core schema transformations adhere strictly to pydantic_core schema protocol definitions.
- Deferred model construction properly wraps validator and serializer access via mock instances without unhandled errors.

## Enforcement

- Verified by: Automated test suites executed against the resolved core library integration.
- Verified by: Static type checking verifying adherence to core schema and handler protocol definitions.
- Verified by: Architecture compliance reviews during pull request inspection.
- Violation handling: Pull requests violating core schema contracts or bypassing the core engine are blocked by automated continuous integration checks.
- Violation handling: Violations detected in codebase audits are scheduled for refactoring into compliant handler callbacks.
- Exception process: Submit an architectural exception request detailing why the core engine schema cannot represent the desired validation behavior.
- Exception process: Provide benchmark data and compatibility analysis demonstrating that alternative handling preserves system stability.
- Exception process: Obtain approval from the core architecture maintainers prior to merging deviations from core schema protocols.