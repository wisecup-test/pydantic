# Adoption of ahash::AHashMap for Core Internal Mappings and Lookup Tables: Before Introducing Updating Collections Backed Ahash

These rules are ALWAYS ACTIVE for all files matching the configured scope (internal key-value lookup trees, mapping structures within validation, serialization, definition caches, and runtime traversal modules operating on trusted internal schemas, identifiers, and field specifications).

### Rules

- **R-AHASH-001** MUST: Before introducing or updating collections backed by ahash::AHashMap, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency version and verify that all invoked constructors and collection methods match that resolved version specification.

### Verify

```bash
# Locate and execute the repository compilation and test runner scripts to confirm modules compile cleanly
cargo check --all-targets
cargo test
# Execute repository benchmark suite to verify throughput metrics
cargo bench
```

**Accept when:**
- All unit and integration test suites pass cleanly across all affected modules.
- Repository static analysis and compilation checks succeed with zero type or collection incompatibility errors.
- Lookup tree and serialization throughput metrics remain within acceptable performance thresholds.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new associative collection instantiations across core modules must be audited via peer review and continuous integration checks.
</enforcement>