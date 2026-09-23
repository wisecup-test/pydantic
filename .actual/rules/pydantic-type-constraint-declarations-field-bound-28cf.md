# Adoption of annotated_types for Canonical Type Constraint Metadata Representation: Type Constraint Declarations Field Boundary Validations

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TC-001** MUST: Type constraint declarations and field boundary validations MUST use `annotated_types` metadata classes as their canonical representation within type annotations and pipeline chains.

### Verify

```bash
# Discover the project test runner and execute the test suite covering field metadata extraction and constraint validation.
# Discover the static analysis suite and verify type checking compliance across modules importing constraint metadata.
```

**Accept when:**
- All field constraint arguments correctly instantiate and serialize into standard metadata descriptors.
- Pipeline constraint methods successfully construct execution chains using standard constraint descriptors without runtime type errors.
- Introspection and annotation reconstruction suites pass without regressions across all field configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>