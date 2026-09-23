# Adoption of pydantic_core for Schema Generation, Validation, and Serialization Core Contracts: Schema Generator Handlers Override Default Reference

These rules are ALWAYS ACTIVE for internal schema generation, signature parsing, and serialization handler modules that construct or consume CoreSchema definitions, and surrogate validator/serializer components that proxy validation execution or handle deferred rebuilding.

### Rules

- **R-PYD-001** MAY: Schema generator handlers MAY override default reference unwrapping behavior when processing annotated metadata to prevent conflicting modifications to definition schemas.
- **R-PYD-002** MANDATORY: The consumer MUST discover the dependency manifest, build tool, and lock/resolution artifact to determine the exact resolved version, and look up official documentation before writing code that uses a versioned library.
- **R-PYD-003** MANDATORY: Implement mock validator and serializer proxies with lazy evaluation triggers that re-invoke the rebuild callable upon first attribute access.
- **R-PYD-004** MANDATORY: Ensure reference resolution logic handles both direct reference mappings and nested definitions wrappers to maintain backward compatibility across schema layouts.

### Verify

```bash
# Discover the project test runner and execute test suites targeting core schema generation, reference resolution, and serialization pipelines.
pytest -k "schema or reference or serialization"

# Discover the repository static analysis and type checking tools and run validation checks across internal handler modules interfacing with core schema protocols.
mypy .
```

**Accept when:**
- All schema generation and reference resolution test suites pass without unresolved reference errors.
- Static type checking confirms all mock proxies and handler callbacks conform to expected core schema interfaces.

<enforcement>
Claude Code MUST NOT skip or defer verification. All schema generation and reference resolution test suites and static type checks must pass successfully.
</enforcement>