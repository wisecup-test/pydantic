# SchemaDict Internal Module Adoption for Schema Dictionary Extraction: Before Consuming Any External Dependency Associated

These rules are ALWAYS ACTIVE for all schema extraction, validator construction, and serializer builder implementations.

### Rules

- **R-SD-001** MUST: Before consuming any external dependency associated with schema extraction bindings, developers MUST locate the project dependency lock artifact and verify the exact resolved version against public API documentation.
- **R-SD-002** MUST: Implement new schema property extraction helpers directly within the SchemaDict trait or struct implementation before consuming them in builders.
- **R-SD-003** MUST: Ensure all schema parsing failures in SchemaDict invoke the common schema error constructor to maintain uniform diagnostic messages.

### Verify

```bash
# Discover and execute the project compilation and linting suite to ensure all schema extraction points conform to SchemaDict contracts.
# Discover and run the project test harness covering validator and serializer builder construction to verify schema parsing compliance.
```

**Accept when:**
- All builder tests pass with all schema extractions performed through SchemaDict.
- Static analysis and linters report zero direct raw dictionary key lookups in validator and serializer construction routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test workflows, automated static analysis, and code review checks.
</enforcement>