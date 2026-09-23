# Adoption of annotated_types for Canonical Type Constraint Metadata Representation: Internal Metadata Inspection Utilities Reconstruct Type

These rules are ALWAYS ACTIVE for internal metadata inspection utilities, field configuration collection, constraint mapping, and validation pipeline step composition.

### Rules

- **R-ADR-001** MUST: Internal metadata inspection utilities MUST reconstruct type expressions by wrapping the core annotation with `annotated_types` metadata objects.

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