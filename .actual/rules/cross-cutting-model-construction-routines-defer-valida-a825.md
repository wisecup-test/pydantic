# Adoption of pydantic_core as Core Schema and Validation Engine: Model Construction Routines Defer Validator Serializer

These rules are ALWAYS ACTIVE for internal modules responsible for model construction, schema gathering, schema generation, and components producing or transforming core schema representations for validation and serialization.

### Rules

- **R-CORE-001** SHOULD: Model construction routines SHOULD defer validator and serializer instantiation using mock wrappers when deferred building is configured.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated test suites and static type checking against the resolved core library integration.
</enforcement>