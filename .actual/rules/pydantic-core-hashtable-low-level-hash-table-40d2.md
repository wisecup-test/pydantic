# Adoption of hashbrown::HashTable for Low-Level Hash Table Management: Modules Utilizing Hashbrown Hashtable Encapsulate Entry

These rules are ALWAYS ACTIVE for internal utility and runtime modules utilizing hashbrown::HashTable that require manual hashing, custom entry representations, or specialized garbage collection traversal.

### Rules

- **R-HASH-001** SHOULD: Modules utilizing hashbrown::HashTable MUST encapsulate entry hashing, resizing, and bucket querying within dedicated wrapper interfaces to prevent raw table operations from leaking across module boundaries.

### Verify

```bash
# Discover and execute project test suite covering hash table data structures and runtime traversal
# Run project linting and formatting checks
```

**Accept when:**
- All unit and integration tests covering hash table operations, capacity growth, and reference traversal pass with zero failures.
- Static analysis and type checks report no errors or unresolved table invariants.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test suites and mandatory peer code review enforce these rules. Pull requests introducing unencapsulated low-level table operations or bypassing lock-file grounding are blocked from merging.
</enforcement>