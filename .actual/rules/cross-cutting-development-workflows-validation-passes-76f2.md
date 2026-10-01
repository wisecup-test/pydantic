# Consolidation of pydantic-core into an in-tree monorepo workspace: Development Workflows Validation Passes Documentation Builds

These rules are ALWAYS ACTIVE for all code and configuration associated with the repository workspace, continuous integration pipelines, and build documentation processes.

### Rules

- **R-PYD-001** MUST: Development workflows, CI validation passes, and documentation builds MUST execute against the in-tree pydantic-core source workspace rather than external package pins.

### Verify

```bash
# Verify uv workspace resolution uses in-tree pydantic-core source
uv tree

# Run validation and test suite covering workspace members
make test

# Build documentation using in-tree sources
./build-docs.sh
```

**Accept when:**
- Workspace tooling resolves pydantic-core locally without network fetch from external package registries.
- CI pipelines pass all validation, documentation, benchmark, and coverage steps against in-tree sources.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: CI job failure if external pydantic-core packages are pulled instead of in-tree sources, or PR rejection when attempting to pin external pydantic-core releases for internal development runs.
</enforcement>