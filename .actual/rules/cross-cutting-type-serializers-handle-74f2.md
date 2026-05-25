# Adopt Rust Type Serializers for Domain Model Serialization: Type Serializers Handle

These rules are ALWAYS ACTIVE for all domain model serialization code in pydantic-core, particularly files in the `serializers/type_serializers/` directory and any code implementing domain-specific type serialization.

### Rules

- **R-SERIALIZERS-001** MUST: Type serializers MUST handle domain-specific types including models, temporal types (timedelta), and sentinel values (missing_sentinel).

### Verify

```bash
# Count type_serializers references in the serializers directory
grep -r "type_serializers" pydantic-core/src/serializers/ | wc -l

# List all type serializer implementation files
find pydantic-core/src/serializers/type_serializers -name "*.rs" -type f

# Run type serializer test suite
cargo test --package pydantic-core --lib serializers::type_serializers
```

**Accept when:**
- Type serializer modules exist in `pydantic-core/src/serializers/type_serializers/` for all domain types requiring specialized serialization
- All type serializers have corresponding test coverage with passing tests
- Grep command returns at least 3 type serializer files (model.rs, timedelta.rs, missing_sentinel.rs)
- All cargo tests pass without errors

<enforcement>
Claude Code MUST NOT skip or defer verification. All type serializer implementations MUST pass the verify commands before accepting changes to domain model serialization logic.
</enforcement>