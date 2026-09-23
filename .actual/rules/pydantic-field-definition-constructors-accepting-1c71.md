# Adoption of annotated_types for Canonical Type Constraint Metadata Representation: Field Definition Constructors Accepting Constraint Parameters

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AT-001** MUST: Field definition constructors accepting constraint parameters MUST map boundary criteria directly to corresponding `annotated_types` descriptors in internal metadata lookups.

### Verify

```bash
# Discover and run the project test runner covering field metadata extraction and constraint validation
# Discover and run the static analysis suite verifying type checking compliance
```

**Accept when:**
- All field constraint arguments correctly instantiate and serialize into standard metadata descriptors.
- Pipeline constraint methods successfully construct execution chains using standard constraint descriptors without runtime type errors.
- Introspection and annotation reconstruction suites pass without regressions across all field configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>