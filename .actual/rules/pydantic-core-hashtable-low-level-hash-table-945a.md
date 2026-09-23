# Adoption of hashbrown::HashTable for Low-Level Hash Table Management: Developers Not Use Hashbrown Hashtable When

These rules are ALWAYS ACTIVE for internal data structures requiring manual hashing, custom entry representations, or specialized garbage collection traversal, and performance-sensitive core routines where high-level map wrappers introduce unacceptable allocation or indirection overhead.

### Rules

- **R-HASH-001** MUST_NOT: Developers MUST NOT use hashbrown::HashTable when standard key-value associative semantics and default collision resolution are sufficient, reserving it strictly for structures requiring manual entry management or custom traversal.
- **R-HASH-002** MANDATORY: Prior to writing code that uses a versioned library, execute lock-version grounding in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use.
- **R-HASH-003** MANDATORY: Construct domain-specific wrappers around hashbrown::HashTable to encapsulate bucket lifecycle, resizing thresholds, and hash generation.
- **R-HASH-004** MANDATORY: Ensure that all table iteration paths correctly handle occupied and vacant bucket states during traversal operations.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute the test suite covering hash table data structures and runtime traversal.
# Run the project linting and formatting checks to verify adherence to codebase standards and type safety constraints.
```

**Accept when:**
- All unit and integration tests covering hash table operations, capacity growth, and reference traversal pass with zero failures.
- Static analysis and type checks report no errors or unresolved table invariants.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing unencapsulated low-level table operations or bypassing lock-file grounding are blocked from merging.
</enforcement>