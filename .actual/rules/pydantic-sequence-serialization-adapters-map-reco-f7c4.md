# Adoption of pydantic_core for Schema Generation, Validation, and Serialization Core Contracts: Sequence Serialization Adapters Map Recognized Collection

These rules are ALWAYS ACTIVE for internal schema generation, signature parsing, and serialization handler modules that construct or consume CoreSchema definitions, as well as surrogate validator and serializer components.

### Rules

- **R-PNC-001** SHOULD: Sequence serialization adapters SHOULD map recognized collection origins through dedicated lookup tables prior to delegating inner item transformations to core schema routines.
- **R-PNC-002** MANDATORY: (Discovery Policy) The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-PNC-003** MANDATORY: (Lock-Version Grounding) Before writing code that uses a versioned library, execute in order: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use.
- **R-PNC-004** MANDATORY: Implement mock validator and serializer proxies with lazy evaluation triggers that re-invoke the rebuild callable upon first attribute access.
- **R-PNC-005** MANDATORY: Ensure reference resolution logic handles both direct reference mappings and nested definitions wrappers to maintain backward compatibility across schema layouts.

### Verify

```bash
# Discover the project test runner and execute test suites targeting core schema generation, reference resolution, and serialization pipelines.
# Discover the repository static analysis and type checking tools and run validation checks across internal handler modules interfacing with core schema protocols.
```

**Accept when:**
- All schema generation and reference resolution test suites pass without unresolved reference errors.
- Static type checking confirms all mock proxies and handler callbacks conform to expected core schema interfaces.

<enforcement>
Claude Code MUST NOT skip or defer verification. All schema generation and reference resolution test suites must pass, static type checking must confirm mock proxies conform to core schema interfaces, and any bypass of core schema interfaces will be blocked during code review.
</enforcement>