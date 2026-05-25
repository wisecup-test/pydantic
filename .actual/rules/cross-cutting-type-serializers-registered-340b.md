# Adopt Type-Specific Serializer Pattern for Public API Contracts: Type Serializers Registered

These rules are ALWAYS ACTIVE for all public API serialization implementations. All type serializers exposed through public APIs MUST conform to these patterns.

### Rules

- **R-SERIALIZER-001** MUST: Type serializers MUST be registered through a central module (mod.rs) that coordinates serializer selection.

### Verify

```bash
# Verify type serializer module structure exists
find . -path '*/serializers/type_serializers/*.rs' -type f | wc -l | grep -E '[0-9]+'

# Verify central coordinator module registers serializers
grep -r 'mod.*serializer' */serializers/type_serializers/mod.rs

# Run serializer tests with coverage validation
cargo test --package pydantic-core --lib serializers::type_serializers
```

**Accept when:**
- All type serializers are implemented in separate modules under type_serializers directory
- Central coordinator module (mod.rs) exists and registers all type-specific serializers
- All serializer tests pass with coverage above 80% for serialization logic

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline fails if new types are serialized without dedicated type serializer. Code review blocks merge if serialization logic is added outside type_serializers module.
</enforcement>