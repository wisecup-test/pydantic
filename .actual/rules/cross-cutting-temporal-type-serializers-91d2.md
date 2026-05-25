# Adopt Pydantic Core Type Serializers for Python Type System Integration: Temporal Type Serializers

These rules are ALWAYS ACTIVE for all Python projects utilizing pydantic-core serialization infrastructure, particularly those implementing type serializers for core Python types including collections (set, frozenset), temporal types (timedelta), and sentinel values.

### Rules

- **R-PYDANTIC-001** MUST: Temporal type serializers (timedelta) MUST handle precision requirements and provide both numeric and ISO 8601 string format outputs.

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
- Temporal type serializers specifically support both numeric and ISO 8601 string format outputs

<enforcement>
Claude Code MUST NOT skip or defer verification. All serializer implementations must pass the cargo test suite and follow established architectural patterns before acceptance.
</enforcement>