# Adopt Pydantic Core Type Serializers for Python Type System Integration: Type Serializers Support

These rules are ALWAYS ACTIVE for all Python projects utilizing pydantic-core serialization infrastructure, particularly implementations of type serializers for core Python types including collections (set, frozenset), temporal types (timedelta), and sentinel values.

### Rules

- **R-PYDANTIC-SERIALIZERS-001** MUST: Type serializers MUST support multiple serialization modes including JSON-compatible output, Python native types, and string representations where applicable.

### Verify

```bash
# Verify type serializer modules exist for core Python types
find pydantic-core/src/serializers/type_serializers -name '*.rs' | xargs grep -l 'impl.*Serializer' | wc -l

# Run comprehensive test suite for all serializer modules
cargo test --package pydantic-core --lib serializers::type_serializers

# Verify consistent serializer struct patterns
grep -r 'pub struct.*Serializer' pydantic-core/src/serializers/type_serializers/
```

**Accept when:**
- All core Python types (set, frozenset, timedelta, sentinel values) have dedicated serializer modules in pydantic-core/src/serializers/type_serializers/
- Each type serializer passes comprehensive test suite covering multiple serialization modes and edge cases
- Serializer implementations follow consistent architectural patterns with proper error handling and context support
- All serializers support JSON-compatible output, Python native types, and string representations as applicable to their type

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verify commands must execute successfully before accepting changes to type serializer implementations. Code review must confirm pattern consistency with reference implementations (set_frozenset.rs, missing_sentinel.rs, timedelta.rs).
</enforcement>