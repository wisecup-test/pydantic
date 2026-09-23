# Adoption of pydantic_core as Core Schema and Validation Engine: Schema Traversal Reference Gathering Routines Operate

These rules are ALWAYS ACTIVE for internal modules responsible for model construction, schema gathering, schema generation, and components producing or transforming core schema representations for validation and serialization.

### Rules

- **R-CORE-001** MUST: Schema traversal and reference gathering routines MUST operate on core schema definitions conforming to pydantic_core core_schema contracts.
- **R-CORE-002** MUST: Execute lock-version grounding in order (find dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for exact version, confirm APIs exist, re-run per dependency at point of use) before writing code that uses a versioned library.
- **R-CORE-003** MUST: Encapsulate schema gathering and reference cleanup in dedicated internal traversal utilities.
- **R-CORE-004** MUST: Ensure deferred build models instantiate mock validators that fail gracefully until explicit rebuilding is triggered.
- **R-CORE-005** MUST: Maintain strict separation between Python-facing metaclass mechanics and low-level schema dictionary construction.

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
Claude Code MUST NOT skip or defer verification. Verified by automated test suites executed against resolved core library integration, static type checking verifying adherence to core schema and handler protocol definitions, and architecture compliance reviews during pull request inspection.
</enforcement>