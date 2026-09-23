# Adoption of annotated_types for Canonical Type Constraint Metadata Representation: Validation Transformation Pipeline Abstractions Compose Constraint

These rules are ALWAYS ACTIVE for validation and transformation pipeline abstractions, field configuration metadata collection, and custom scalar and temporal type constraint declarations.

### Rules

- **R-AT-001** MUST: Validation and transformation pipeline abstractions MUST compose constraint steps using `annotated_types` predicate and boundary structures rather than custom ad-hoc predicates.

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
Claude Code MUST NOT skip or defer verification. All pull requests or code implementations must strictly adhere to standard `annotated_types` metadata protocols and pass all associated automated test and static analysis suites.
</enforcement>