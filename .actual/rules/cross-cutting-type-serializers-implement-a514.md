# Adopt Rust Type Serializers for Domain Model Serialization: Type Serializers Implement

These rules are ALWAYS ACTIVE for all domain model serialization code in pydantic-core, particularly files in the `serializers/type_serializers/` directory and any new domain types requiring specialized serialization.

### Rules

- **R-RUST-TS-001** MAY: Type serializers MAY implement performance optimizations specific to their domain type characteristics.

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
- All cargo tests pass without errors or warnings

<enforcement>
Claude Code MUST NOT skip or defer verification. All type serializer implementations MUST pass the verify commands before acceptance. Code review MUST check for proper type serializer implementation patterns. CI pipeline MUST run cargo tests and block merge on test failure.
</enforcement>