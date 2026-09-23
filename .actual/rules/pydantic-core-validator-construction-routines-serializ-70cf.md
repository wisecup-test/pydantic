# SchemaDict Internal Module Adoption for Schema Dictionary Extraction: Validator Construction Routines Serializer Builders Schema

These rules are ALWAYS ACTIVE for all validator construction routines, serializer builders, and schema configuration extractors.

### Rules

- **R-SCHEMADICT-001** MUST: All validator construction routines, serializer builders, and schema configuration extractors MUST parse incoming schema dictionaries exclusively through SchemaDict contracts rather than performing raw dictionary key lookups.

### Verify

```bash
# Discover and execute the project compilation, linting suite, and test harness
# to ensure all schema extraction points conform to SchemaDict contracts.
# (Commands must be discovered from the project repository configuration)
```

**Accept when:**
- All builder tests pass with all schema extractions performed through SchemaDict.
- Static analysis and linters report zero direct raw dictionary key lookups in validator and serializer construction routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test workflows and automated static analysis will block pull requests containing raw dictionary lookups in builder routines.
</enforcement>