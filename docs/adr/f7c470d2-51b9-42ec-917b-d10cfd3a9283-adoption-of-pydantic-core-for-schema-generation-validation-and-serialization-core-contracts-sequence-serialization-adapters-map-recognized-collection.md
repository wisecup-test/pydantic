# Adoption of pydantic_core for Schema Generation, Validation, and Serialization Core Contracts: Sequence Serialization Adapters Map Recognized Collection

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Context

- The high-level data validation library requires a distinct separation between high-level Python type introspection and low-level schema compilation, validation, and serialization routines.
- Core schema structures, recursive reference resolutions, and execution mechanics are delegated to pydantic_core rather than maintained as native high-level object hierarchies.
- Internal schema generation modules, signature builders, and serializers must interface with CoreSchema representations, SchemaValidator, and SchemaSerializer protocols.
- Deferred model rebuilding and lazy schema evaluation require proxy structures such as MockValSer and MockCoreSchema that intercept method invocations until schema rebuild triggers succeed.

## Problem Statement

Maintaining monolithic validation and serialization logic inside high-level Python structures leads to substantial runtime overhead and complicates recursive schema resolution. Without a standardized contract interfacing with a specialized validation engine, internal schema generators and serialization adapters risk diverging across custom type hierarchies, circular references, and dynamic imports.

## Decision

1. SHOULD: Sequence serialization adapters SHOULD map recognized collection origins through dedicated lookup tables prior to delegating inner item transformations to core schema routines.

## Policy Block

- SHOULD Sequence serialization adapters SHOULD map recognized collection origins through dedicated lookup tables prior to delegating inner item transformations to core schema routines.

In scope:
- Internal schema generation, signature parsing, and serialization handler modules that construct or consume CoreSchema definitions.
- Surrogate validator and serializer components that proxy validation execution or handle deferred rebuilding.

Out of scope:
- High-level end-user public API entry points that operate strictly on validated output objects without interacting with schema generation internals.
- Standalone utility modules that have no dependency on data validation, serialization, or type introspection pipelines.

Exceptions:
- EXC-20-001: A module requires lightweight metadata extraction without constructing a full CoreSchema or initializing a SchemaValidator instance.

## Rationale

- Centralizing low-level schema representations within pydantic_core isolates execution performance concerns from high-level Python AST and type analysis.
- Standardizing reference resolution routines ensures recursive models and definitions can be resolved consistently across schema generators without infinite recursions.
- Providing MockValSer and MockCoreSchema abstractions facilitates lazy schema construction and robust error dispatch when dependent models fail rebuild prerequisites.

## Consequences

Positive:
- Strict separation of concerns between declarative Python model definitions and low-level schema validation and serialization execution.
- Consistent handling of schema reference definitions and recursive type definitions through unified resolution protocols.
- Reliable lazy rebuilding mechanisms that prevent premature validation failures during dynamic module loading.

Negative:
- Tighter architectural coupling between high-level internal modules and the low-level pydantic_core schema dictionary layout.
- Increased debugging complexity when navigating between high-level schema generation handlers and low-level mock or validator proxies.

## Alternatives

- Implement pure Python validation and serialization routines directly within model definitions without a dedicated core schema engine. (rejected)
  Rejected because: Lacks the execution speed of specialized core validation routines and leads to severe architectural fragmentation across custom serializer and validator methods.
  When valid: Small prototype libraries with minimal serialization overhead and no recursive model resolution requirements.
- Eagerly evaluate and compile all validation schemas at module import time without mock surrogates or deferred rebuild hooks. (rejected)
  Rejected because: Causes circular import crashes and premature initialization failures when model definitions reference unresolved forward annotations.
  When valid: Static type structures where all types and models are completely declared in advance with no circular dependencies.

## Risks

- Schema format drift between high-level generator dictionaries and low-level core validator expectations across dependency upgrades.
  Mitigation: Strict lock-version verification and automated integration test suites validating CoreSchema generation contracts.
  Owner: Core Engineering Team
- Uncaught LookupError during recursive schema reference lookup if a circular definition is improperly unwrapped.
  Mitigation: Enforce explicit definition reference resolution guards and structured error formatting in reference handling callbacks.
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
- Implement mock validator and serializer proxies with lazy evaluation triggers that re-invoke the rebuild callable upon first attribute access.
- Ensure reference resolution logic handles both direct reference mappings and nested definitions wrappers to maintain backward compatibility across schema layouts.

## Continuation Context


Verify commands:
- Discover the project test runner and execute test suites targeting core schema generation, reference resolution, and serialization pipelines.
- Discover the repository static analysis and type checking tools and run validation checks across internal handler modules interfacing with core schema protocols.

Accept when:
- All schema generation and reference resolution test suites pass without unresolved reference errors.
- Static type checking confirms all mock proxies and handler callbacks conform to expected core schema interfaces.

## Enforcement

- Verified by: Automated continuous integration suites running unit and integration tests across schema generators.
- Verified by: Mandatory architectural peer review for any modification touching core schema handlers or validation proxies.
- Violation handling: Pull requests introducing direct validation workarounds that bypass core schema interfaces will be blocked during code review.
- Violation handling: Schema generation regressions causing unresolved reference errors will fail automated continuous integration pipelines.
- Exception process: Submit an architectural exception request detailing why direct schema handling is required, subject to lead maintainer review and documentation.