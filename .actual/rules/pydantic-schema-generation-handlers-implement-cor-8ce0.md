# Adoption of pydantic_core for Schema Generation, Validation, and Serialization Core Contracts: Schema Generation Handlers Implement Coreschema Contract

These rules are ALWAYS ACTIVE for all internal schema generation, signature parsing, and serialization handler modules that construct or consume CoreSchema definitions, and surrogate validator and serializer components.

### Rules

- **R-PCOL-001** MUST: Schema generation handlers MUST implement the CoreSchema contract conventions and resolve reference definitions through designated schema reference resolution methods when handling definition reference structures.

### Verify

```bash
# Discover and execute test suites targeting core schema generation, reference resolution, and serialization pipelines using the project's test runner.
# Discover the repository static analysis and type checking tools and run validation checks across internal handler modules interfacing with core schema protocols.
```

**Accept when:**
- All schema generation and reference resolution test suites pass without unresolved reference errors.
- Static type checking confirms all mock proxies and handler callbacks conform to expected core schema interfaces.

<enforcement>
Verified by automated continuous integration suites running unit and integration tests across schema generators and mandatory architectural peer reviews. Violation handling blocks pull requests bypassing core schema interfaces or causing unresolved reference errors.
</enforcement>