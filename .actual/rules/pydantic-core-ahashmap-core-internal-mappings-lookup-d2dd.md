# Adoption of ahash::AHashMap for Core Internal Mappings and Lookup Tables: Components Not Use Ahash Ahashmap Persistent

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AHASH-001** MUST_NOT: Components MUST NOT use ahash::AHashMap for persistent or cross-process serialization formats where deterministic cross-platform hash values or cryptographic collision resistance against untrusted adversarial key inputs is strictly required.

### Verify

```bash
# Locate and execute the repository compilation and test runner scripts to confirm that all modules using ahash::AHashMap compile without type or borrow check errors.
# Execute the repository benchmark suite to verify that lookup tree and serialization throughput metrics remain within acceptable performance thresholds.
cargo test
cargo bench
```

**Accept when:**
- All unit and integration test suites pass cleanly across all affected modules.
- Repository static analysis and compilation checks succeed with zero type or collection incompatibility errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>