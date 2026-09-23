# Adoption of ahash::AHashMap for Core Internal Mappings and Lookup Tables: Components Combine Ahash Ahashmap Auxiliary Memory

These rules are ALWAYS ACTIVE for all internal key-value lookup trees, validation modules, serialization routines, definition caches, and runtime traversal components operating on trusted internal schemas, identifiers, and field specifications.

### Rules

- **R-AHASH-001** MAY: Components MAY combine ahash::AHashMap with auxiliary memory-optimization wrappers or reference-counted containers when storing composite schema structures or managing object lifetime traversals.
- **R-AHASH-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-AHASH-003** MANDATORY: Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo; (2) Identify the build tool from the manifest; (3) Inspect the repository lock or resolution artifact to determine the exact resolved version; (4) Look up official documentation, changelog, or public API reference for that exact version; (5) Confirm every API, class, or function used exists in that exact version's documentation; (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.
- **R-AHASH-004** MANDATORY: Replace standard associative collection types with ahash::AHashMap across internal caching and lookup structures, verifying that referenced keys implement the required hashing traits.
- **R-AHASH-005** MANDATORY: When traversing references during runtime garbage collection cycles or building field serialization hierarchies, pre-allocate map capacity where collection sizes are known in advance.
- **R-AHASH-006** MANDATORY: Confine ahash::AHashMap usage to internal schema definitions, validated fields, and trusted runtime metadata to prevent hash collision vulnerabilities from untrusted adversary inputs.
- **R-AHASH-007** MANDATORY: Enforce strict dependency resolution verification via lock artifacts before adopting upgraded collection releases.

### Verify

```bash
# Locate and execute the repository compilation and test runner scripts to confirm modules compile without type or borrow check errors
# (Repository-specific build/test command, e.g., cargo test --all)

# Execute the repository benchmark suite to verify lookup tree and serialization throughput metrics remain within acceptable performance thresholds
# (Repository-specific benchmark command, e.g., cargo bench)
```

**Accept when:**
- All unit and integration test suites pass cleanly across all affected modules.
- Repository static analysis and compilation checks succeed with zero type or collection incompatibility errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Peer code reviews will reject pull requests introducing default standard library hash maps for internal lookup or caching hot paths, and continuous integration checks failing benchmark or compilation standards must be resolved prior to merging.
</enforcement>