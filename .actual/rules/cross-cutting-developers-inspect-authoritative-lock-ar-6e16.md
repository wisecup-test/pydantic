# Adoption of pydantic_core as Core Schema and Validation Engine: Developers Inspect Authoritative Lock Artifact Repository

These rules are ALWAYS ACTIVE for internal modules responsible for model construction, schema gathering, schema generation, components producing or transforming core schema representations, and mocking structures managing deferred model initialization and validation rebuilding.

### Rules

- **R-PCO-001** MUST: Developers MUST inspect the authoritative lock artifact in the repository to resolve the exact dependency version before integrating with the validation engine.

### Verify

```bash
# Discover and execute the project verification script from the root repository configuration to run all core schema test suites.
# Discover and execute the static type analysis tool across internal schema generation modules to ensure compliance with core schema protocols.
```

**Accept when:**
- All schema generation, model construction, and reference resolution tests pass against the resolved validation engine.
- Static analysis confirms all core schema transformations adhere strictly to pydantic_core schema protocol definitions.
- Deferred model construction properly wraps validator and serializer access via mock instances without unhandled errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>