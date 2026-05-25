# Adopt Rust Type Serializers for Domain Model Serialization: Each Distinct Domain

These rules are ALWAYS ACTIVE for all domain models requiring serialization in pydantic-core, custom domain types with specialized serialization requirements, temporal domain types, sentinel and special marker values, and complex nested domain model structures.

### Rules

- **R-RUST-SERIALIZERS-001** MUST: Each distinct domain type requiring specialized serialization logic MUST have its own type serializer module.

### Verify

```bash
# Count type serializer references in the codebase
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
- Cargo test suite passes with no failures

<enforcement>
Claude Code MUST NOT skip or defer verification. All type serializer implementations MUST pass the cargo test suite before acceptance. Code review MUST block merge if domain types lack appropriate type serializers. Architecture review MUST flag domain models with inline serialization logic as violations.
</enforcement>