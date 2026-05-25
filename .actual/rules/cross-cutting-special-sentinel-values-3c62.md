# Adopt Type-Specific Serializer Pattern for Public API Contracts: Special Sentinel Values

These rules are ALWAYS ACTIVE for all public API serialization implementations and type serializers exposed through public APIs.

### Rules

- **R-SENTINEL-001** MUST: Special sentinel values (e.g., missing, undefined) MUST have dedicated serializer implementations to ensure API contract clarity.

### Verify

```bash
# Verify type serializer module structure exists
find . -path '*/serializers/type_serializers/*.rs' -type f | wc -l | grep -E '[0-9]+'

# Verify central coordinator module registers serializers
grep -r 'mod.*serializer' */serializers/type_serializers/mod.rs

# Run serializer tests with coverage verification
cargo test --package pydantic-core --lib serializers::type_serializers
```

**Accept when:**
- All type serializers are implemented in separate modules under type_serializers directory
- Central coordinator module (mod.rs) exists and registers all type-specific serializers
- All serializer tests pass with coverage above 80% for serialization logic
- Sentinel value serializers (missing, undefined) have dedicated implementations
- Integration tests validate serialization consistency across all public endpoints

<enforcement>
Claude Code MUST NOT skip or defer verification. All type serializers for sentinel values MUST be implemented in dedicated modules and registered through the central coordinator before merging.
</enforcement>