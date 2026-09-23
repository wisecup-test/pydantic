# Adoption of std::sync::OnceLock for Thread-Safe Lazy Initialization: Consumers Evaluate Read Access Frequency Use

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Static and lazily evaluated data structures across serialization routines, URL parsers, and core library exports require thread-safe one-time initialization.
- Previous synchronization approaches often relied on external lazy-initialization crates or complex mutex guards that increased dependency footprint or runtime locking contention.
- The standard synchronization module provides std::sync::OnceLock as a native primitive for thread-safe, write-once cell initialization with lock-free read performance once populated.
- A consistent initialization primitive across core modules standardizes concurrency guarantees and avoids ad-hoc synchronization mechanisms.

## Problem Statement

Shared static configuration, format registries, and cached lookup structures across core library components require deferred initialization on first access without compromising thread safety. Without a standardized synchronization primitive, modules risk data races, inefficient repetitive locking, or reliance on external synchronization crates.

## Decision

1. SHOULD: Consumers SHOULD evaluate read access frequency and use get on std::sync::OnceLock instances after initialization to minimize synchronization overhead in performance-critical execution paths.

## Policy Block

- SHOULD Consumers SHOULD evaluate read access frequency and use get on std::sync::OnceLock instances after initialization to minimize synchronization overhead in performance-critical execution paths.

In scope:
- Core library modules requiring lazy or one-time initialization of static data structures
- Thread-safe shared caches and schema serialization lookup tables

Out of scope:
- Thread-local data structures that are not shared across thread boundaries
- Mutable state structures requiring ongoing updates after initial assignment

## Rationale

- Standardizing on std::sync::OnceLock provides deterministic, thread-safe one-time initialization across core modules without introducing external synchronization crate dependencies.
- Read operations on initialized std::sync::OnceLock cells provide near zero-cost access without recurring lock contention across concurrent calls.
- The native API surface ensures uniform error handling and initialization semantics across serialization, URL processing, and core library entry points.

## Consequences

Positive:
- Thread safety is guaranteed across all lazy-initialized shared state without custom synchronization logic.
- External dependency footprint for lazy static initialization is eliminated.
- Post-initialization read operations achieve optimal throughput with minimal synchronization overhead.

Negative:
- Initial population of the cell can incur blocking latency across concurrent threads competing to execute the initialization closure.
- Values stored within the synchronization cell cannot be mutated or invalidated after initial assignment.

## Alternatives

- External lazy initialization crates and macros (rejected)
  Rejected because: Introducing third-party synchronization crates increases external dependency maintenance overhead when equivalent capabilities exist within standard library modules.
  When valid: When targeting compilation environments lacking standard library synchronization primitives.
- Mutual exclusion locks guarding optional static state (rejected)
  Rejected because: Mutual exclusion locks impose repeated acquisition overhead on every read access even after initialization is complete.
  When valid: When state requires recurring mutation or cache eviction after initial population.

## Risks

- Deadlocks caused by re-entrant initialization where the initialization closure recursively accesses the same cell
  Mitigation: Enforce strict boundaries ensuring initialization closures do not invoke functions that attempt to read or initialize the same cell instance
  Owner: Core Engineering Team
- Blocking contention during expensive initialization operations under high concurrent load
  Mitigation: Keep initialization logic lightweight and defer heavy non-essential computations outside the one-time cell initialization block
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
- Instantiate std::sync::OnceLock instances in static or long-lived structures and initialize via get_or_init during first runtime request.
- Ensure types held inside std::sync::OnceLock fulfill necessary thread-safety marker traits for safe sharing across threads.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting routines to verify std::sync::OnceLock usage complies with synchronization policies.
- Execute the repository test suite with concurrency and race detection flags enabled to validate thread safety under parallel execution.

Accept when:
- All static analysis and linting checks pass without warnings regarding unsafe static mutation or non-standard synchronization mechanisms.
- All concurrency test suites complete successfully without detecting data races or deadlocks during lazy initialization.

## Enforcement

- Verified by: Automated static analysis checks in the continuous integration pipeline
- Verified by: Peer code review verification of synchronization primitives and closure safety
- Violation handling: Pull requests introducing raw mutable statics or non-standard synchronization mechanisms will be rejected during automated checks and code review
- Exception process: Exceptions require formal architectural review and documented justification demonstrating why write-once semantics are insufficient.