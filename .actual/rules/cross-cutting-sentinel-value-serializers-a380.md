# Adopt Pydantic Core Type Serializers for Python Type System Integration: Sentinel Value Serializers

These rules are ALWAYS ACTIVE for all Python projects utilizing pydantic-core serialization infrastructure, particularly those implementing type serializers for core Python types including collections (set, frozenset), temporal types (timedelta), and sentinel values.

### Rules

- **R-PYDANTIC-SERIALIZERS-001** MUST: Sentinel value serializers (missing, undefined) MUST distinguish between explicit None values and absent/undefined values in serialization output.

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
- Sentinel value serializers correctly distinguish between explicit None and absent/undefined values in all output formats

<enforcement>
Claude Code MUST NOT skip or defer verification. All serializer implementations MUST pass the cargo test suite and follow established patterns before approval. Code review MUST verify that sentinel value serializers properly handle the distinction between None and undefined values.
</enforcement>