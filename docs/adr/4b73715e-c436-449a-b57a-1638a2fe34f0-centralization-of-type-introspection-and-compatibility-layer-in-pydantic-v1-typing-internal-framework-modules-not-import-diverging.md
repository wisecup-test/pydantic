# Centralization of Type Introspection and Compatibility Layer in pydantic.v1.typing: Internal Framework Modules Not Import Diverging

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Runtime typing capabilities and structural introspection primitives vary across supported platform execution environments.
- Core modules responsible for configuration, error construction, and annotated model definitions require consistent protocol contracts and dictionary validation.
- Direct reliance on built-in typing constructs in individual modules leads to fragmented compatibility shims and duplicated runtime checks.
- The library establishes a centralized internal typing module to unify type introspection helpers, fallback compatibility shims, and shared protocol definitions.

## Problem Statement

Directly consuming platform typing primitives across disparate core framework modules leads to fragmented compatibility workarounds and divergent runtime behavior across distinct execution platforms. Without a centralized typing abstraction, modules evaluating annotations, error templates, and configuration options must repeatedly implement conditional logic to normalize structural type definitions and protocol declarations.

## Decision

1. MUST_NOT: Internal framework modules MUST NOT import diverging platform-specific typing constructs directly when an equivalent shim is maintained within the centralized typing layer.

## Policy Block

- MUST_NOT Internal framework modules MUST NOT import diverging platform-specific typing constructs directly when an equivalent shim is maintained within the centralized typing layer.

In scope:
- Internal core library modules responsible for configuration, error definitions, and type introspection.
- All internal protocol specifications and structural dictionary validation routines.

Out of scope:
- External client code consuming standard library types independently of framework internals.
- Standalone schema generation routines decoupled from core framework execution contexts.

Exceptions:
- EXC-20-001: A module requires typing constructs that are strictly invariant across all supported runtime environments and require no conditional fallback.

## Rationale

- Centralizing typing abstractions in pydantic.v1.typing prevents fragmentation across core modules and eliminates duplicated conditional imports for runtime compatibility.
- Consolidated protocols such as SchemaExtraCallable and structured definitions such as ConfigDict guarantee uniform contracts across model configuration and serialization layers.
- Centralized introspection predicates such as is_typeddict and is_legacy_typeddict ensure that differences in dictionary field optionality across execution platforms are consistently resolved.

## Consequences

Positive:
- Provides a unified source of truth for runtime type compatibility and protocol specifications across core library modules.
- Eliminates duplicated type compatibility logic and divergent edge-case handling across model configuration and error handling layers.
- Ensures consistent type evaluation and structural dictionary introspection across differing execution platforms.

Negative:
- Increases coupling of internal core subsystems to a custom typing abstraction module.
- Introduces maintenance overhead whenever upstream runtime platform typing specifications change.

## Alternatives

- Direct unshimmed imports of platform typing primitives across all core modules (rejected)
  Rejected because: Produces divergent runtime behavior and introspection failures across varying interpreter environments due to evolving structural typing specifications.
  When valid: Only valid when targeting a single immutable runtime environment where typing behaviors are guaranteed invariant.
- Per-module ad-hoc compatibility guards and inline exception handling (rejected)
  Rejected because: Results in duplicated inspection routines, divergent edge-case handling, and increased maintenance overhead across the library.
  When valid: Acceptable only in isolated standalone scripts that do not participate in shared framework contracts.

## Risks

- Typing shims may fail to anticipate structural changes introduced in future runtime platforms.
  Mitigation: Validate type introspection contracts and shims across all supported runtime platforms within automated continuous integration test matrices.
  Owner: Core Library Maintainers
- Developers may bypass the centralized typing module and import non-portable typing primitives directly.
  Mitigation: Implement automated import checking and static analysis rules to flag direct imports of non-portable typing primitives.
  Owner: Core Library Maintainers

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Consolidate all new protocol classes, generic type definitions, and environment-dependent type shims within the centralized typing module before consuming them across subsystems.
- Verify that structural introspection utilities properly distinguish required and optional fields across all supported runtime environments.

## Continuation Context


Verify commands:
- Discover the project test runner configuration and execute the complete test suite across all supported runtime environments.
- Discover the static analysis and type checking configuration in the repository manifest and execute type verification on the core modules.

Accept when:
- All unit and integration test suites pass without runtime type errors across all supported execution environments.
- Static type analysis validates that all cross-module protocols and configuration dictionaries conform to shared typing declarations.

## Enforcement

- Verified by: Automated static type checkers and linter rules governing internal module imports.
- Verified by: Continuous integration test matrices executed across all supported runtime platforms.
- Verified by: Peer code review on modifications touching core module type definitions.
- Violation handling: Pull requests importing divergent platform typing constructs directly when a shared shim exists are rejected.
- Violation handling: Static analysis or test suite failures in continuous integration gate merges.
- Exception process: Submit an architecture exception request detailing why the shared typing layer cannot provide the required type primitive, subject to maintainer approval.