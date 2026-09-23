# Adoption of enum_dispatch for Static Polymorphic Dispatch: Core Execution Hot Paths Not Use

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Core validation and garbage collection traversal routines require polymorphic execution over numerous distinct variant types.
- Dynamic dispatch through heap-allocated trait objects incurs pointer indirection and inhibits compiler inlining on critical execution paths.
- Manual match arm dispatch creates repetitive maintenance overhead as new variant structures are introduced across trait contracts.

## Problem Statement

High-throughput validation and garbage collection traversal require polymorphic method invocation across dozens of distinct variant types without incurring runtime vtable overhead or accumulating repetitive manual dispatch boilerplate.

## Decision

1. MUST_NOT: Core execution hot paths MUST NOT use heap-allocated dynamic trait objects for polymorphism when variant sets can be statically enumerated.

## Policy Block

- MUST_NOT Core execution hot paths MUST NOT use heap-allocated dynamic trait objects for polymorphism when variant sets can be statically enumerated.

In scope:
- Polymorphic method dispatch across closed sets of types implementing shared traits.
- Core validation logic and garbage collection traversal contracts requiring zero runtime dynamic dispatch overhead.

Out of scope:
- Open extension points where downstream consumers must provide arbitrary third-party implementations not known at compile time.
- Internal utility types with trivial single-implementation traits where enum wrapping is unnecessary.

Exceptions:
- EXC-20-001: Dynamic trait objects are strictly required for third-party pluggable extensibility that cannot be represented in a closed enum.

## Rationale

- The codebase requires zero-cost static polymorphism in tight validation loops and garbage collection traversal paths where dynamic dispatch introduces unacceptable pointer indirection and inhibits compiler inlining.
- Adopting enum_dispatch eliminates repetitive, hand-written match arms across variant types while retaining the runtime performance of direct function calls.
- Evidence across validator modules and runtime garbage collection traversal contracts demonstrates enum_dispatch provides unified trait contracts with minimal structural overhead.

## Consequences

Positive:
- Eliminates dynamic vtable lookups and heap boxing overhead in high-throughput validation and garbage collection traversal paths.
- Reduces boilerplate code by automatically generating variant match arms across trait implementations.
- Enables compiler inlining across statically dispatched enum variant methods.

Negative:
- Increases compilation overhead due to macro expansion across enum variant definitions.
- Restricts polymorphism to closed variant sets, preventing open-ended third-party extension without modifying the central enum.

## Alternatives

- Dynamic dispatch using heap-allocated trait objects (rejected)
  Rejected because: Incurs heap allocation overhead and prevents compiler inlining due to vtable pointer indirection on hot paths.
  When valid: When trait implementations must be open to downstream third-party plugins that cannot be known at compile time.
- Manual enum match dispatch implementations (rejected)
  Rejected because: Creates extensive, error-prone boilerplate across all enum variants whenever trait signatures or variant lists change.
  When valid: When custom dispatch logic or heterogeneous parameter transformations are required between variants.

## Risks

- Enum variant bloat causing excessive macro expansion and code generation, leading to slower compilation times.
  Mitigation: Group related variants into domain-specific sub-enums and monitor build times during continuous integration runs.
  Owner: Core engineering team
- Accidental macro signature divergence between trait definitions and enum annotations.
  Mitigation: Rely on automated compiler diagnostics and build verification pipelines to catch mismatched trait bindings at compile time.
  Owner: Core engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Apply the enum_dispatch attribute to the trait definition before applying it to the corresponding enum definition that aggregates the variant structs.
- Ensure every variant struct implements all functions of the annotated trait prior to compiling the enclosing dispatch enum.

## Continuation Context


Verify commands:
- Discover and run the project's static analysis and compilation verification tasks to confirm macro expansions and trait bindings resolve without errors.
- Execute the project's automated test suite to validate that static dispatch calls execute accurately across all enum variants.

Accept when:
- All trait methods invoke the correct variant implementations through static dispatch without runtime errors.
- Compilation passes without macro expansion warnings or missing trait implementation errors across all configured build targets.

## Enforcement

- Verified by: Continuous integration compile-time type checking and test suite execution.
- Verified by: Peer code reviews ensuring polymorphic paths do not introduce unneeded dynamic trait objects.
- Violation handling: Automated build failure if trait bindings or macro annotations fail compile-time validation.
- Violation handling: Rejection of pull requests introducing dynamic trait objects in performance-critical validation paths.
- Exception process: Formal review and approval by core engine maintainers with accompanying benchmark justification.