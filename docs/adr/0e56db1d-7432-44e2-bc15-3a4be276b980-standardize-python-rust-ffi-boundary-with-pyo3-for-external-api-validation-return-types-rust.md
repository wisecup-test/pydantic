# Standardize Python-Rust FFI Boundary with PyO3 for External API Validation: Return Types Rust

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ACTIVE for all Python-Rust FFI boundary implementations in pydantic-core and similar validation libraries exposing external APIs.

## Context

- The pydantic-core library implements high-performance validation logic in Rust while exposing a Python API, requiring a robust Foreign Function Interface (FFI) boundary
- Pattern detected across 40 files with 89.47% confidence, primarily in validators, serializers, and core library modules handling Python-Rust interop
- PyO3 framework provides type-safe bindings between Python and Rust, enabling efficient data validation while maintaining Python's dynamic interface
- External API consumers expect Pythonic interfaces while benefiting from Rust's performance and type safety for validation operations
- Testing and mocking capabilities must work across the FFI boundary to support both unit testing and integration testing scenarios

## Problem Statement

How do we design and implement a consistent, type-safe, and performant FFI boundary between Python and Rust for validation libraries that expose external APIs, while ensuring testability, maintainability, and adherence to Python ecosystem conventions?

## Decision

1. MUST: Return types from Rust validators MUST use strongly-typed enums (return_enums) rather than dynamic Python objects to maintain type safety

## Policy Block

- MUST Return types from Rust validators MUST use strongly-typed enums (return_enums) rather than dynamic Python objects to maintain type safety

In scope:
- All Rust modules in pydantic-core exposing Python APIs via PyO3
- Validators, serializers, and configuration objects crossing the FFI boundary
- Input handling, return enums, and validation state management
- Python garbage collection integration for Rust-owned objects
- Type serializers for standard Python types (UUID, datetime, URL, etc.)

Out of scope:
- Pure Rust internal implementation details not exposed to Python
- Python-only code that doesn't interact with Rust extensions
- Third-party libraries' FFI implementations outside pydantic-core
- Platform-specific system calls or OS-level APIs

Exceptions:
- EXC-001: Performance-critical hot paths require unsafe FFI operations
- EXC-002: Legacy compatibility requires dynamic type handling

## Rationale

- Pattern detected with 89.47% confidence across 40 files indicates this is a well-established architectural approach in the codebase
- PyO3 provides the most mature and type-safe FFI framework for Python-Rust interop, reducing memory safety issues and improving maintainability
- Consistent FFI patterns across validators, serializers, and core modules enable easier testing, debugging, and onboarding of new contributors
- Strong typing at the FFI boundary catches errors at compile time rather than runtime, improving reliability for external API consumers

## Consequences

Positive:
- Type-safe FFI boundary reduces runtime errors and improves API reliability for external consumers
- PyO3's automatic Python garbage collection integration prevents memory leaks in long-running applications
- Consistent patterns across 40+ files improve code maintainability and reduce cognitive load for contributors
- Performance benefits of Rust implementation while maintaining Pythonic API surface for ease of adoption
- Testability through standard Python testing frameworks enables comprehensive test coverage

Negative:
- PyO3 dependency adds compilation complexity and increases build times for the project
- FFI boundary introduces serialization overhead for data crossing between Python and Rust
- Debugging across language boundaries requires expertise in both Python and Rust ecosystems
- Breaking changes in PyO3 framework may require significant refactoring effort

## Alternatives

- Use ctypes or cffi for Python-Rust FFI with C-compatible ABI (rejected)
  Rejected because: Lacks type safety, requires manual memory management, and provides no automatic Python garbage collection integration. Higher risk of memory leaks and segmentation faults.
  When valid: Only for interfacing with legacy C libraries where PyO3 bindings are not feasible
- Implement pure Python validation without Rust optimization (rejected)
  Rejected because: Performance requirements for validation at scale necessitate compiled language implementation. Pure Python would be 10-100x slower for complex validation scenarios.
  When valid: Prototyping new validators or for projects where performance is not critical
- Use Cython for Python-C++ integration with validation logic in C++ (rejected)
  Rejected because: Rust provides better memory safety guarantees than C++ without garbage collection overhead. PyO3 ecosystem is more mature for Rust-Python interop than Cython for C++.
  When valid: When existing C++ validation libraries must be integrated and rewriting in Rust is not feasible

## Risks

- PyO3 version upgrades may introduce breaking changes requiring extensive refactoring across 40+ files
  Mitigation: Pin PyO3 version in Cargo.toml, establish upgrade testing protocol, maintain compatibility layer for gradual migration
  Owner: Core maintainers and FFI working group
- FFI boundary performance overhead may become bottleneck for high-frequency validation operations
  Mitigation: Implement batching for bulk validations, use zero-copy techniques where possible, profile and optimize hot paths
  Owner: Performance engineering team
- Complex error propagation across FFI boundary may obscure root causes in production debugging
  Mitigation: Implement structured error types with full context preservation, add tracing spans across boundary, maintain error code documentation
  Owner: Engineering team and SRE

## Implementation Notes

- Use #[pyclass] for all Rust structs exposed to Python and #[pymethods] for method implementations to ensure proper FFI binding generation
- Implement Drop trait carefully for Rust types holding Python references to prevent reference cycles and memory leaks
- Leverage PyO3's GILGuard and py.allow_threads() for CPU-intensive validation operations to avoid blocking Python's Global Interpreter Lock
- Create integration tests that exercise the full FFI boundary with realistic validation scenarios and edge cases
- Document Python type hints for all exposed APIs to maintain IDE support and type checking capabilities for library users

## Continuation Context


Verify commands:
- grep -r '#\[pyclass\]\|#\[pymethods\]' pydantic-core/src/ | wc -l
- cargo test --package pydantic-core --lib -- validators serializers --nocapture
- python -m pytest tests/ -k 'ffi or boundary or validator' --tb=short
- rg 'use pyo3::' pydantic-core/src/ --count-matches

Accept when:
- All validator and serializer modules use PyO3 macros for FFI bindings (grep count > 80)
- Integration tests pass with >95% coverage for FFI boundary code paths
- No memory leaks detected in long-running validation stress tests (valgrind or similar)
- Python type stubs (.pyi files) exist for all exposed Rust APIs with complete type annotations

## Enforcement

- Verified by: Automated CI checks for PyO3 macro usage in all new validator/serializer implementations
- Verified by: Code review checklist requiring FFI safety review for changes touching Python-Rust boundary
- Verified by: Integration test suite with mandatory FFI boundary coverage requirements
- Verified by: Static analysis tools (clippy, mypy) configured to catch FFI-related issues
- Violation handling: CI pipeline fails if new FFI code lacks proper PyO3 annotations or test coverage
- Violation handling: Pull requests blocked until FFI safety review is completed by designated reviewers
- Violation handling: Runtime warnings logged when deprecated FFI patterns are detected
- Violation handling: Quarterly audits of FFI boundary code with remediation plans for violations
- Exception process: Submit exception request to architecture review board with performance data or technical justification
- Exception process: Document exception in ADR amendments with approval signatures and expiration date
- Exception process: Create tracking issue for technical debt with plan to eliminate exception in future release
- Exception process: Require additional test coverage and documentation for approved exceptions