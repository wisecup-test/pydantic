# Adoption of annotated_types for Canonical Type Constraint Metadata Representation: Engineers Discover Ecosystem Lock Artifact Verify

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Engineers MUST discover the ecosystem lock artifact and verify the exact resolved version of `annotated_types` before declaring new metadata integrations.

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