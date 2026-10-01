# Consolidation of pydantic-core into an in-tree monorepo workspace: Workspace Package Resolution Configurations Resolve Pydantic

These rules are ALWAYS ACTIVE for all repository workspace configuration files, continuous integration workflows, core testing, and build scripts interacting with pydantic-core.

### Rules

- **R-PDS-001** MUST: Workspace package resolution configurations MUST resolve pydantic-core directly from local repository workspace members.

### Verify

```bash
# Discover and execute workspace dependency resolution commands to confirm in-tree source binding for all workspace members
uv lock --check
uv sync --locked
# Run joint test suite and build scripts across workspace components
make test
make docs
```

**Accept when:**
- Workspace tooling resolves pydantic-core locally without network fetch from external package registries.
- CI pipelines pass all validation, documentation, benchmark, and coverage steps against in-tree sources.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>