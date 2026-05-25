# Adopt Pydantic Core Type Serializers for Python Type System Integration: Core Python Types

These rules are ALWAYS ACTIVE for all Python projects utilizing pydantic-core serialization infrastructure, particularly implementations of type serializers for core Python types including collections (set, frozenset), temporal types (timedelta), and sentinel values.

### Rules

- **R-PYDANTIC-001** MUST: All core Python types requiring specialized serialization logic MUST implement dedicated type serializer modules within the pydantic-core serializers infrastructure.

### Verify

```bash
# Verify type serializer modules exist
find pydantic-core/src/serializers/type_serializers -name '*.rs' | xargs grep -l 'impl.*Serializer' | wc -l

# Run comprehensive test suite for all serializer modules
cargo test --package pydantic-core --lib serializers::type_serializers

# Verify serializer struct definitions follow pattern
grep -r 'pub struct.*Serializer' pydantic-core/src/serializers/type_serializers/
```

**Accept when:**
- All core Python types (set, frozenset, timedelta, sentinel values) have dedicated serializer modules in pydantic-core/src/serializers/type_serializers/
- Each type serializer passes comprehensive test suite covering multiple serialization modes and edge cases
- Serializer implementations follow consistent architectural patterns with proper error handling and context support
- New type serializers follow the established pattern in set_frozenset.rs, missing_sentinel.rs, and timedelta.rs as reference implementations
- Each type serializer module includes comprehensive unit tests covering edge cases, nested serialization, and all supported output modes
- Serializer implementations handle serialization context parameters to enable format-specific behavior (e.g., JSON vs Python dict output)

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline blocks merge if serializer tests fail or coverage drops below threshold. Code review identifies pattern deviations and requests refactoring before approval.
</enforcement>