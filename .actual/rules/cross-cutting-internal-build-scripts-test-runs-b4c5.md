# Consolidation of pydantic-core into an in-tree monorepo workspace: Internal Build Scripts Test Runs Not

These rules are ALWAYS ACTIVE for repository workspace configuration, continuous integration workflows, documentation build scripts, and core testing/benchmark configurations.

### Rules

- **R-CORE-001** MUST_NOT: Internal build scripts and test runs MUST NOT fetch external pydantic-core release artifacts from package registries for local workspace tasks.

### Verify

```bash
# Verify workspace dependency resolution uses in-tree sources
uv tree
# Run the joint test suite and build checks
make test
```

**Accept when:**
- Workspace tooling resolves pydantic-core locally without network fetch from external package registries.
- CI pipelines pass all validation, documentation, benchmark, and coverage steps against in-tree sources.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes must be verified against in-tree workspace sources without pulling external package artifacts.
</enforcement>