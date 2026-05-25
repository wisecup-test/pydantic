# Adopt Pydantic Core Type Serializers for Python Type System Integration: Core Python Types

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ACTIVE for all Python projects utilizing pydantic-core serialization infrastructure. It governs the implementation of type serializers for core Python types including collections (set, frozenset), temporal types (timedelta), and sentinel values.

## Context

- The pydantic-core library requires specialized serializers for Python's built-in types to enable efficient data validation and serialization across different output formats (JSON, Python dict, etc.)
- Type serializers for collections (set, frozenset), temporal types (timedelta), and sentinel values (missing/undefined) represent fundamental building blocks that must handle edge cases and maintain type safety during serialization
- The Rust-based implementation in pydantic-core provides performance-critical serialization paths that must integrate seamlessly with Python's type system while maintaining compatibility with various serialization targets
- Pattern detected across 3 core serializer implementations (set_frozenset.rs, missing_sentinel.rs, timedelta.rs) with 89.47% confidence, indicating a consistent architectural approach to type serialization

## Problem Statement

Python's rich type system includes collections, temporal types, and sentinel values that require specialized serialization logic to maintain type fidelity, handle edge cases, and provide efficient conversion across multiple output formats. Without a standardized type serializer architecture, inconsistent handling of these types would lead to data loss, type confusion, and performance degradation in validation and serialization pipelines.

## Decision

1. MUST: All core Python types requiring specialized serialization logic MUST implement dedicated type serializer modules within the pydantic-core serializers infrastructure

## Policy Block

- MUST All core Python types requiring specialized serialization logic MUST implement dedicated type serializer modules within the pydantic-core serializers infrastructure

In scope:
- Core Python built-in types requiring specialized serialization (set, frozenset, timedelta, sentinel values)
- Pydantic-core serialization infrastructure and type serializer modules
- JSON, Python dict, and string serialization output formats
- Nested type serialization where collections contain other serializable types

Out of scope:
- User-defined custom types not part of Python's standard library
- Third-party library types requiring external serialization adapters
- Database-specific serialization formats (SQL, binary protocols)
- Serialization of non-data types (functions, classes, modules)

Exceptions:
- EXC-001: Legacy Python 2 compatibility requires alternative serialization approaches for types with changed semantics
- EXC-002: Performance profiling demonstrates that pure Python implementation provides equivalent or better performance for specific type serializers

## Rationale

- The detected pattern across set_frozenset.rs, missing_sentinel.rs, and timedelta.rs demonstrates a consistent architectural approach to handling diverse Python types with specialized serialization requirements
- Rust-based implementation provides significant performance benefits for high-frequency serialization operations while maintaining type safety and correctness
- Modular type serializer architecture enables independent testing, maintenance, and extension of serialization logic for each type family
- Supporting multiple serialization modes (JSON, Python native, string) ensures compatibility with diverse downstream consumers and use cases

## Consequences

Positive:
- Consistent and predictable serialization behavior across all core Python types in pydantic-core
- High-performance serialization through Rust implementation reduces overhead in validation-heavy applications
- Modular architecture simplifies testing, debugging, and extension of type-specific serialization logic
- Clear separation of concerns between type serializers enables parallel development and maintenance

Negative:
- Rust implementation creates barrier to entry for Python-only contributors and increases build complexity
- Maintaining multiple serialization modes per type increases code surface area and testing requirements
- Type serializer proliferation may lead to inconsistent patterns if not carefully governed
- Performance optimization in Rust may obscure serialization logic making debugging more difficult

## Alternatives

- Use Python's built-in json.JSONEncoder with custom default() method for all type serialization (rejected)
  Rejected because: Pure Python approach lacks performance characteristics required for high-throughput validation scenarios and provides insufficient control over serialization modes
  When valid: Acceptable for low-performance requirements or prototyping where build complexity must be minimized
- Implement single monolithic serializer with type dispatch rather than modular type-specific serializers (rejected)
  Rejected because: Monolithic approach reduces maintainability, increases coupling, and makes testing more complex as type-specific logic becomes intertwined
  When valid: May be appropriate for very small type sets (< 5 types) where modularity overhead exceeds benefits
- Leverage existing serialization libraries (msgpack, orjson) as serialization backend (rejected)
  Rejected because: External dependencies lack pydantic-specific semantics (sentinel values, validation context) and reduce control over serialization behavior
  When valid: Could be used as optimization for specific output formats while maintaining pydantic-core as primary interface

## Risks

- Rust implementation complexity may slow down bug fixes and feature additions due to limited Rust expertise in contributor base
  Mitigation: Maintain comprehensive test suite, provide detailed documentation for Rust serializer patterns, and establish mentorship program for Rust contributions
  Owner: Pydantic core maintainers
- Type serializer proliferation without governance may lead to inconsistent patterns and technical debt
  Mitigation: Establish serializer implementation guidelines, require ADR for new type serializers, and conduct regular architecture reviews
  Owner: Engineering team
- Breaking changes in Python's type system semantics across versions may require significant serializer refactoring
  Mitigation: Implement version-specific test suites, monitor Python enhancement proposals (PEPs), and maintain compatibility matrix documentation
  Owner: Engineering team

## Implementation Notes

- New type serializers should follow the established pattern in set_frozenset.rs, missing_sentinel.rs, and timedelta.rs as reference implementations
- Each type serializer module should include comprehensive unit tests covering edge cases, nested serialization, and all supported output modes
- Serializer implementations must handle serialization context parameters to enable format-specific behavior (e.g., JSON vs Python dict output)
- Performance benchmarks should be included for new serializers to validate Rust implementation benefits and identify optimization opportunities

## Continuation Context


Verify commands:
- find pydantic-core/src/serializers/type_serializers -name '*.rs' | xargs grep -l 'impl.*Serializer' | wc -l
- cargo test --package pydantic-core --lib serializers::type_serializers
- grep -r 'pub struct.*Serializer' pydantic-core/src/serializers/type_serializers/

Accept when:
- All core Python types (set, frozenset, timedelta, sentinel values) have dedicated serializer modules in pydantic-core/src/serializers/type_serializers/
- Each type serializer passes comprehensive test suite covering multiple serialization modes and edge cases
- Serializer implementations follow consistent architectural patterns with proper error handling and context support

## Enforcement

- Verified by: Automated CI pipeline runs cargo test suite for all serializer modules
- Verified by: Code review checklist verifies new serializers follow established patterns
- Verified by: Static analysis tools check for consistent error handling and context usage
- Violation handling: CI pipeline blocks merge if serializer tests fail or coverage drops below threshold
- Violation handling: Code review identifies pattern deviations and requests refactoring before approval
- Violation handling: Architecture review board evaluates significant deviations and may require ADR amendment
- Exception process: Submit exception request with technical justification and performance/compatibility data
- Exception process: Core maintainer reviews exception against policy scope and alternatives
- Exception process: Approved exceptions documented in ADR amendments with rationale and constraints