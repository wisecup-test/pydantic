# Adoption of pydantic_core as Core Schema and Validation Engine: Internal Schema Generation Data Validation Layers

These rules are ALWAYS ACTIVE for internal modules responsible for model construction, schema gathering, schema generation, components producing or transforming core schema representations, and mocking structures managing deferred model initialization and validation rebuilding.

### Rules

- **R-PDC-001** MUST: Internal schema generation and data validation layers MUST delegate core validation and serialization execution to pydantic_core schema structures.
- **R-PDC-002** MUST: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-PDC-003** MUST: Follow lock-version grounding before writing code that uses a versioned library: find the manifest, identify the build tool, inspect the lock/resolution artifact for the exact version, look up official docs for that exact version, confirm every API exists, and re-run for version-sensitive behavior.
- **R-PDC-004** MUST: Encapsulate schema gathering and reference cleanup in dedicated internal traversal utilities.
- **R-PDC-005** MUST: Ensure deferred build models instantiate mock validators that fail gracefully until explicit rebuilding is triggered.
- **R-PDC-006** MUST: Maintain strict separation between Python-facing metaclass mechanics and low-level schema dictionary construction.

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
Claude Code MUST NOT skip or defer verification. Pull requests violating core schema contracts or bypassing the core engine are blocked by automated continuous integration checks.
</enforcement>