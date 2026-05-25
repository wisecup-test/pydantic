# Adopt Rust Type Serializers for Domain Model Serialization: New Domain Types

These rules are ALWAYS ACTIVE for all domain models requiring custom serialization in pydantic-core, including temporal types, sentinel values, and complex nested structures.

### Rules

- **R-RUST-TS-001** SHOULD: New domain types requiring custom serialization SHOULD follow the established pattern of creating dedicated type serializer modules in `pydantic-core/src/serializers/type_serializers/`.

### Verify

```bash
# Count type_serializers references in the codebase
grep -r "type_serializers" pydantic-core/src/serializers/ | wc -l

# List all type serializer files
find pydantic-core/src/serializers/type_serializers -name "*.rs" -type f

# Run type serializer tests
cargo test --package pydantic-core --lib serializers::type_serializers
```

**Accept when:**
- Type serializer modules exist in `pydantic-core/src/serializers/type_serializers/` for all domain types requiring specialized serialization
- All type serializers have corresponding test coverage with passing tests
- Grep command returns at least 3 type serializer files (model.rs, timedelta.rs, missing_sentinel.rs)
- Cargo test suite passes without errors

<enforcement>
Claude Code MUST NOT skip or defer verification. All new domain types requiring serialization MUST be validated against these rules before acceptance.
</enforcement>