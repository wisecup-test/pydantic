# Adoption of pydantic_core for Schema Generation, Validation, and Serialization Core Contracts: Before Introducing Updating Any Interaction Core

These rules are ALWAYS ACTIVE for internal schema generation modules, signature builders, serializers, surrogate validator and serializer components that construct or consume CoreSchema definitions.

### Rules

- **R-CORE-001** MUST: Before introducing or updating any interaction with the core validation engine dependency, the engineering consumer MUST inspect the repository lock artifact to determine the exact resolved dependency version and verify that all referenced interfaces exist in the published documentation for that version.

### Verify

```bash
# Discover the project test runner and execute test suites targeting core schema generation, reference resolution, and serialization pipelines.
# Discover the repository static analysis and type checking tools and run validation checks across internal handler modules interfacing with core schema protocols.
```

**Accept when:**
- All schema generation and reference resolution test suites pass without unresolved reference errors.
- Static type checking confirms all mock proxies and handler callbacks conform to expected core schema interfaces.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration suites running unit and integration tests across schema generators and mandatory architectural peer review for any modification touching core schema handlers or validation proxies.
</enforcement>