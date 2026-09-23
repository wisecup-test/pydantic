# Adoption of pydantic_core as Core Schema and Validation Engine: Schema Generation Handlers Normalize Definition References

These rules are ALWAYS ACTIVE for internal modules responsible for model construction, schema gathering, schema generation, components producing or transforming core schema representations for validation and serialization, and mocking structures managing deferred model initialization and validation rebuilding.

### Rules

- **R-PDS-001** SHOULD: Schema generation handlers SHOULD normalize definition references through centralized handler callbacks before submitting them to the core engine.
- **R-PDS-002** MANDATORY: The consumer MUST discover dependency management, build tools, exact resolved versions, and verification scripts from the project repository configurations and lock artifacts.
- **R-PDS-003** MANDATORY: Encapsulate schema gathering and reference cleanup in dedicated internal traversal utilities.
- **R-PDS-004** MANDATORY: Ensure deferred build models instantiate mock validators that fail gracefully until explicit rebuilding is triggered.
- **R-PDS-005** MANDATORY: Maintain strict separation between Python-facing metaclass mechanics and low-level schema dictionary construction.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory and enforced by automated continuous integration checks and architecture compliance reviews.
</enforcement>