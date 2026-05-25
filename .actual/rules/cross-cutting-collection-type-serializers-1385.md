# Adopt Pydantic Core Type Serializers for Python Type System Integration: Collection Type Serializers

These rules are ALWAYS ACTIVE for all Python projects utilizing pydantic-core serialization infrastructure, specifically governing the implementation of type serializers for core Python types including collections (set, frozenset), temporal types (timedelta), and sentinel values.

### Rules

- **R-PYDANTIC-CORE-001** MUST: Collection type serializers (set, frozenset) MUST preserve element ordering constraints and handle nested serialization of contained elements.
- **R-PYDANTIC-CORE-002** MUST: Each type serializer module MUST include comprehensive unit tests covering edge cases, nested serialization, and all supported output modes (JSON, Python dict, string).
- **R-PYDANTIC-CORE-003** MUST: Serializer implementations MUST handle serialization context parameters to enable format-specific behavior (e.g., JSON vs Python dict output).
- **R-PYDANTIC-CORE-004** SHOULD: New type serializers SHOULD follow the established pattern in set_frozenset.rs, missing_sentinel.rs, and timedelta.rs as reference implementations.
- **R-PYDANTIC-CORE-005** SHOULD: Performance benchmarks SHOULD be included for new serializers to validate Rust implementation benefits and identify optimization opportunities.
- **R-PYDANTIC-CORE-006** MAY: Legacy Python 2 compatibility MAY require alternative serialization approaches for types with changed semantics (EXC-001).
- **R-PYDANTIC-CORE-007** MAY: Pure Python implementation MAY be used when performance profiling demonstrates equivalent or better performance for specific type serializers (EXC-002).

### Verify

```bash
# Count type serializer implementations
find pydantic-core/src/serializers/type_serializers -name '*.rs' | xargs grep -l 'impl.*Serializer' | wc -l

# Run serializer test suite
cargo test --package pydantic-core --lib serializers::type_serializers

# Verify serializer struct definitions
grep -r 'pub struct.*Serializer' pydantic-core/src/serializers/type_serializers/
```

**Accept when:**
- All core Python types (set, frozenset, timedelta, sentinel values) have dedicated serializer modules in pydantic-core/src/serializers/type_serializers/
- Each type serializer passes comprehensive test suite covering multiple serialization modes and edge cases
- Serializer implementations follow consistent architectural patterns with proper error handling and context support
- All serializer tests pass in CI pipeline with coverage above configured threshold

<enforcement>
Claude Code MUST NOT skip or defer verification. All verify commands MUST execute successfully before accepting changes to type serializer implementations. Code review MUST verify new serializers follow established patterns. CI pipeline MUST block merge if serializer tests fail or coverage drops below threshold.
</enforcement>