# Adopt Pydantic Core Type Serializers for Python Type System Integration: Type Serializers Implement

These rules are ALWAYS ACTIVE for all Python projects utilizing pydantic-core serialization infrastructure, particularly implementations of type serializers for core Python types including collections (set, frozenset), temporal types (timedelta), and sentinel values.

### Rules

- **R-PYDANTIC-SERIALIZERS-001** MAY: Type serializers MAY implement custom error handling for invalid or unsupported type conversions.

### Verify

```bash
# Verify type serializer modules exist for core Python types
find pydantic-core/src/serializers/type_serializers -name '*.rs' | xargs grep -l 'impl.*Serializer' | wc -l

# Run comprehensive test suite for all serializer modules
cargo test --package pydantic-core --lib serializers::type_serializers

# Verify consistent serializer structure across implementations
grep -r 'pub struct.*Serializer' pydantic-core/src/serializers/type_serializers/
```

**Accept when:**
- All core Python types (set, frozenset, timedelta, sentinel values) have dedicated serializer modules in pydantic-core/src/serializers/type_serializers/
- Each type serializer passes comprehensive test suite covering multiple serialization modes and edge cases
- Serializer implementations follow consistent architectural patterns with proper error handling and context support

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated CI pipeline runs cargo test suite for all serializer modules. Code review checklist verifies new serializers follow established patterns. Static analysis tools check for consistent error handling and context usage. CI pipeline blocks merge if serializer tests fail or coverage drops below threshold.
</enforcement>