# Adoption of ahash::AHashMap for Core Internal Mappings and Lookup Tables: Internal Components Pass Ahash Ahashmap Instances

These rules are ALWAYS ACTIVE for internal key-value lookup trees, mapping structures within validation, serialization, definition caches, and runtime traversal modules.

### Rules

- **R-AHASH-001** SHOULD: Internal components SHOULD pass ahash::AHashMap instances across internal module boundaries whenever shared lookup state or cached field metadata is transferred between validation and serialization layers.
- **R-AHASH-002** MANDATORY: The consumer MUST derive tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-AHASH-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use for version-sensitive behavior.
- **R-AHASH-004** MANDATORY: Replace standard associative collection types with ahash::AHashMap across internal caching and lookup structures, verifying that referenced keys implement the required hashing traits.
- **R-AHASH-005** MANDATORY: When traversing references during runtime garbage collection cycles or building field serialization hierarchies, pre-allocate map capacity where collection sizes are known in advance.
- **R-AHASH-006** MANDATORY: Confine ahash::AHashMap usage to internal schema definitions, validated fields, and trusted runtime metadata.

### Verify

```bash
# Locate and execute the repository compilation and test runner scripts to confirm that all modules using ahash::AHashMap compile without type or borrow check errors.
# Execute the repository benchmark suite to verify that lookup tree and serialization throughput metrics remain within acceptable performance thresholds.
```

**Accept when:**
- All unit and integration test suites pass cleanly across all affected modules.
- Repository static analysis and compilation checks succeed with zero type or collection incompatibility errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Code reviews will reject pull requests introducing default standard library hash maps for internal lookup or caching hot paths. Continuous integration checks failing benchmark or compilation standards must be resolved prior to merging.
</enforcement>