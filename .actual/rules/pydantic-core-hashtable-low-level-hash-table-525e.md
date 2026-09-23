# Adoption of hashbrown::HashTable for Low-Level Hash Table Management: Before Implementing Modifying Structures Hashbrown Hashtable

These rules are ALWAYS ACTIVE for internal data structures requiring manual hashing, custom entry representations, or specialized garbage collection traversal using hashbrown::HashTable.

### Rules

- **R-HASH-001** MUST: Before implementing or modifying structures using hashbrown::HashTable, developers MUST discover the dependency declaration artifact and the associated lock artifact to resolve the exact pinned library version in accordance with the lock-version grounding policy.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute the test suite covering hash table data structures and runtime traversal.
# Run the project linting and formatting checks to verify adherence to codebase standards and type safety constraints.
```

**Accept when:**
- All unit and integration tests covering hash table operations, capacity growth, and reference traversal pass with zero failures.
- Static analysis and type checks report no errors or unresolved table invariants.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing unencapsulated low-level table operations or bypassing lock-file grounding are blocked from merging.
</enforcement>