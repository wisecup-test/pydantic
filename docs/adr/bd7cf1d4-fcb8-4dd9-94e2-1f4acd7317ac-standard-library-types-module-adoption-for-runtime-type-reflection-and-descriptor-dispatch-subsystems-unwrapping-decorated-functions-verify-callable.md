# Standard Library types Module Adoption for Runtime Type Reflection and Descriptor Dispatch: Subsystems Unwrapping Decorated Functions Verify Callable

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Core validation and data modeling frameworks require runtime introspection of types, callable signatures, and class attributes to construct dynamic validation schemas
- Standard class attributes including classmethods, staticmethods, and property descriptors require transparent binding interception without disrupting normal attribute access or setter delegation
- Generic model instantiation requires dynamic class generation and type variable mapping that must be cached deterministically to prevent redundant class creation

## Problem Statement

Dynamic data validation frameworks must introspect callable signatures, wrap class methods, and specialize generic models at runtime without mutating original function signatures prematurely or breaking descriptor protocol mechanics. Relying on external reflection dependencies increases package weight and creates downstream version conflicts, while naive attribute wrapping breaks classmethod and property descriptor delegation.

## Decision

1. SHOULD: Subsystems unwrapping decorated functions SHOULD verify callable interfaces and unwrap nested descriptors safely before passing functions to schema generators.

## Policy Block

- SHOULD Subsystems unwrapping decorated functions SHOULD verify callable interfaces and unwrap nested descriptors safely before passing functions to schema generators.

In scope:
- Core framework modules performing dynamic class construction, method decoration, generic parameterization, or runtime type inspection

Out of scope:
- Static schema generation logic and external protocol declarations that do not interact with dynamic runtime callables or class descriptors

## Rationale

- Adopting the standard library types module ensures lightweight, dependency-free type introspection and dynamic dispatch across core modules
- Encapsulating decorated methods in dedicated descriptor proxies guarantees that descriptor binding semantics remain intact across inheritance hierarchies
- Caching generic parameterizations via deterministic key derivation prevents redundant class synthesis and memory degradation

## Consequences

Positive:
- Eliminates third-party reflection dependencies, maintaining a lightweight runtime footprint for core validation logic
- Preserves native descriptor protocol behaviors for classmethods, staticmethods, and computed property fields across model inheritance hierarchies
- Provides deterministic type parameterization and submodel caching for generic data structures

Negative:
- Requires maintaining custom descriptor proxy classes and function unwrapping logic to preserve metadata transparency
- Dynamic type parameterization requires careful cache size management to avoid unbounded memory retention under high permutation counts
- Complex descriptor interactions must account for behavioral differences across runtime descriptor protocol implementations

## Alternatives

- Adopting external third-party reflection and proxy libraries (rejected)
  Rejected because: Introduces external runtime dependencies, increases packaging footprint, and creates potential compatibility conflicts across downstream consumer environments
  When valid: When developing application-level services where packaging constraints are secondary to third-party framework ecosystems
- Direct callable replacement without descriptor proxy abstraction (rejected)
  Rejected because: Breaks descriptor protocol mechanics for classmethods, staticmethods, and properties when accessed as class attributes during model construction
  When valid: When wrapping standalone unbound functions that do not participate in class binding or descriptor protocols
- Standard library types module integration with internal descriptor proxies and caching containers (accepted)
  Rejected because: N/A
  When valid: Always valid for core framework runtime reflection, generic parameterization, and decorator instrumentation

## Risks

- Descriptor proxying can inadvertently obscure callable signatures or introspection metadata for third-party inspection tools
  Mitigation: Implement explicit attribute forwarding and unwrapping helpers on descriptor proxy instances to preserve metadata transparency
  Owner: Core Framework Engineering Team
- Unbounded caching of dynamic generic specializations could result in memory growth under high permutation counts
  Mitigation: Utilize bounded cache containers with eviction policies for generic type specializations
  Owner: Core Framework Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- When constructing descriptor proxies, ensure descriptor protocol methods such as attribute binding and setter delegation operate transparently on wrapped functions
- Ensure type reflection operations in generic parameterization handle composite arguments and unhashable structures cleanly before attempting cache key resolution

## Continuation Context


Verify commands:
- Discover the repository build configuration and execute the primary automated test suite governing internal reflection, decorators, and generic models
- Discover and run the project type checking verification task to ensure all descriptor proxies and generic type wrappers satisfy static typing contracts
- Discover and run the project linting and structural analysis suites to detect prohibited reflection patterns or unregistered dependencies

Accept when:
- All automated tests covering generic model creation, class decorators, and type adapters pass cleanly without reflection errors
- Static type analysis across decorator definitions and generic submodel generators completes with zero diagnostic errors
- Dynamic descriptor proxies preserve transparent attribute access and binding semantics across model hierarchies

## Enforcement

- Verified by: Continuous integration automated test suites covering dynamic model generation and decorator execution
- Verified by: Static type checker validation runs across core model and decorator definitions
- Verified by: Peer code review for internal reflection and descriptor proxy modifications
- Violation handling: Block pull requests that introduce external reflection dependencies or bypass established descriptor proxy structures
- Violation handling: Flag regressions in generic type caching or descriptor binding during automated continuous integration runs
- Exception process: Submit an architectural review request demonstrating why standard types reflection or descriptor proxies cannot satisfy the specific dispatch requirement
- Exception process: Obtain explicit approval from core framework maintainers prior to introducing alternative dynamic dispatch mechanisms