# Consolidation of pydantic-core into an in-tree monorepo workspace: Automated Continuous Integration Suites Execute Tests

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PCORE-001** MUST: Automated continuous integration suites MUST execute tests, benchmarks, and coverage checks jointly across Pydantic and in-tree pydantic-core components.

### Verify

```bash
# Discover and execute workspace dependency resolution commands to confirm in-tree source binding for all workspace members
uv sync --all-extras
# Run the joint test suite, documentation build, and benchmark execution scripts across workspace components
make test
make docs
```

**Accept when:**
- Workspace tooling resolves pydantic-core locally without network fetch from external package registries.
- CI pipelines pass all validation, documentation, benchmark, and coverage steps against in-tree sources.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>