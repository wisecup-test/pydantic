# Adopt Pydantic Core Type Serializers for Python Type System Integration: Serializers Provide Configurable

These rules are ALWAYS ACTIVE for all Python projects utilizing pydantic-core serialization infrastructure, particularly implementations of type serializers for core Python types including collections (set, frozenset), temporal types (timedelta), and sentinel values.

### Rules

- **R-SERIALIZERS-001** SHOULD: Serializers SHOULD provide configurable behavior through serialization context to handle format-specific requirements (JSON, Python dict, string output modes).

### Verify

```bash
# Verify type serializer modules exist for core Python types
find pydantic-core/src/serializers/type_serializers -name '*.rs' | xargs grep -l 'impl.*Serializer' | wc -l

# Run comprehensive test suite for all serializer modules
cargo test --package pydantic-core --lib serializers::type_serializers

# Verify serializer struct definitions follow pattern
grep -r 'pub struct.*Serializer' pydantic-core/src/serializers/type_serializers/
```

**Accept when:**
- All core Python types (set, frozenset, timedelta, sentinel values) have dedicated serializer modules in `pydantic-core/src/serializers/type_serializers/`
- Each type serializer passes comprehensive test suite covering multiple serialization modes and edge cases
- Serializer implementations follow consistent architectural patterns with proper error handling and context support
- Serializers accept and utilize serialization context parameters to enable format-specific behavior

<enforcement>
Claude Code MUST NOT skip or defer verification. All verify commands MUST execute successfully before accepting changes to serializer implementations. Code review MUST verify new serializers follow established patterns and handle serialization context appropriately.
</enforcement>