# Adopt Pydantic Core Type Serializers for Python Type System Integration: Type Serializers Implemented

These rules are ALWAYS ACTIVE for all Python projects utilizing pydantic-core serialization infrastructure, particularly implementations of type serializers for core Python types including collections (set, frozenset), temporal types (timedelta), and sentinel values.

### Rules

- **R-PYDANTIC-SERIALIZERS-001** SHOULD: Type serializers SHOULD be implemented in Rust for performance-critical paths while maintaining Python API compatibility.

### Verify

```bash
# Verify type serializer modules exist
find pydantic-core/src/serializers/type_serializers -name '*.rs' | xargs grep -l 'impl.*Serializer' | wc -l

# Run comprehensive test suite for all serializer modules
cargo test --package pydantic-core --lib serializers::type_serializers

# Verify consistent serializer structure
grep -r 'pub struct.*Serializer' pydantic-core/src/serializers/type_serializers/
```

**Accept when:**
- All core Python types (set, frozenset, timedelta, sentinel values) have dedicated serializer modules in pydantic-core/src/serializers/type_serializers/
- Each type serializer passes comprehensive test suite covering multiple serialization modes and edge cases
- Serializer implementations follow consistent architectural patterns with proper error handling and context support
- Serializers handle nested type serialization where collections contain other serializable types
- All supported output formats (JSON, Python dict, string) are tested for each serializer

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated CI pipeline blocks merge if serializer tests fail or coverage drops below threshold. Code review identifies pattern deviations and requests refactoring before approval.
</enforcement>