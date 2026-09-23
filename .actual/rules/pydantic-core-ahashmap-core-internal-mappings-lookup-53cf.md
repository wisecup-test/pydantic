# Adoption of ahash::AHashMap for Core Internal Mappings and Lookup Tables: Core Internal Data Structures Requiring Associative

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AHASH-001** MUST: Core internal data structures requiring associative key-value mapping, definition caches, validation lookup trees, and serialization field registries MUST use ahash::AHashMap rather than default standard library hash maps.

### Verify

```bash
# Locate and execute the repository compilation and test runner scripts to confirm compilation and test pass status
cargo check --all-targets
cargo test
# Execute the repository benchmark suite
cargo bench
```

**Accept when:**
- All unit and integration test suites pass cleanly across all affected modules.
- Repository static analysis and compilation checks succeed with zero type or collection incompatibility errors.
- Lookup tree and serialization throughput metrics remain within acceptable performance thresholds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration build checks and peer code reviews enforce these requirements; violations will cause pull request rejection or build failure.
</enforcement>