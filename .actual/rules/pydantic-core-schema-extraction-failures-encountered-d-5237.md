# SchemaDict Internal Module Adoption for Schema Dictionary Extraction: Schema Extraction Failures Encountered During Schemadict

These rules are ALWAYS ACTIVE for all code handling schema dictionary extraction, validator construction, and serializer initialization.

### Rules

- **R-SCHEMADICT-001** MUST: Schema extraction failures encountered during SchemaDict parsing MUST propagate through the centralized schema error construction pipeline.
- **R-SCHEMADICT-002** MUST: Implement new schema property extraction helpers directly within the SchemaDict trait or struct implementation before consuming them in builders.
- **R-SCHEMADICT-003** MUST: Ensure all schema parsing failures in SchemaDict invoke the common schema error constructor to maintain uniform diagnostic messages.

### Verify

```bash
# Discover and execute project compilation, linting suite, and test harness covering validator/serializer builder construction to verify schema parsing compliance
```

**Accept when:**
- All builder tests pass with all schema extractions performed through SchemaDict.
- Static analysis and linters report zero direct raw dictionary key lookups in validator and serializer construction routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing raw dictionary lookups in builder routines will be blocked from merging until refactored to use SchemaDict.
</enforcement>