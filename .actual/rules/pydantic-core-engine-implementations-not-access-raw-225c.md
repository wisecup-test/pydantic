# SchemaDict Internal Module Adoption for Schema Dictionary Extraction: Engine Implementations Not Access Raw Dictionary

These rules are ALWAYS ACTIVE for all core schema dictionary extraction, validator construction, and serializer initialization code.

### Rules

- **R-SCHEMADICT-001** MUST_NOT: Engine implementations MUST NOT access raw dictionary keys directly when constructing validator or serializer components from schema definitions.

### Verify

```bash
# Discover and execute the project compilation and linting suite to ensure all schema extraction points conform to SchemaDict contracts.
# Discover and run the project test harness covering validator and serializer builder construction to verify schema parsing compliance.
```

**Accept when:**
- All builder tests pass with all schema extractions performed through SchemaDict.
- Static analysis and linters report zero direct raw dictionary key lookups in validator and serializer construction routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests containing raw dictionary lookups in builder routines will be blocked from merging until refactored to use SchemaDict.
</enforcement>