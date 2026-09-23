# Adoption of pydantic_core for Schema Generation, Validation, and Serialization Core Contracts: Internal Proxy Mocker Classes Validators Serializers

These rules are ALWAYS ACTIVE for internal schema generation, signature parsing, serialization handler modules, and surrogate validator or serializer components that proxy validation execution or handle deferred rebuilding.

### Rules

- **R-PYD-001** MUST_NOT: Internal proxy mocker classes for validators and serializers MUST NOT perform eager schema compilation if the target schema requires deferred rebuilding, and MUST raise designated user errors when rebuild triggers fail.

### Verify

```bash
# Discover the project test runner and execute test suites targeting core schema generation, reference resolution, and serialization pipelines.
# Discover the repository static analysis and type checking tools and run validation checks across internal handler modules interfacing with core schema protocols.
```

**Accept when:**
- All schema generation and reference resolution test suites pass without unresolved reference errors.
- Static type checking confirms all mock proxies and handler callbacks conform to expected core schema interfaces.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>