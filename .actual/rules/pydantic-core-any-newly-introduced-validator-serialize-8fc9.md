# SchemaDict Internal Module Adoption for Schema Dictionary Extraction: Any Newly Introduced Validator Serializer Prebuilt

These rules are ALWAYS ACTIVE for any newly introduced validator, serializer, or prebuilt definition requiring schema dictionary traversal.

### Rules

- **R-SCHEMADICT-001** SHOULD: Any newly introduced validator, serializer, or prebuilt definition requiring schema dictionary traversal SHOULD consume SchemaDict for configuration extraction.

### Verify

```bash
# Discover and execute the project compilation, linting suite, and test harness covering validator and serializer builder construction to verify schema parsing compliance and ensure zero direct raw dictionary key lookups.
```

**Accept when:**
- All builder tests pass with all schema extractions performed through SchemaDict.
- Static analysis and linters report zero direct raw dictionary key lookups in validator and serializer construction routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test workflows and automated static analysis/code review checks.
</enforcement>