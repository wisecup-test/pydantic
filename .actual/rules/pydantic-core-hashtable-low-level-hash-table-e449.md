# Adoption of hashbrown::HashTable for Low-Level Hash Table Management: When Hash Table Entries Contain References

These rules are ALWAYS ACTIVE for internal data structures requiring manual hashing, custom entry representations, or specialized garbage collection traversal, and performance-sensitive core routines where high-level map wrappers introduce unacceptable allocation or indirection overhead.

### Rules

- **R-HASH-001** MUST: When hash table entries contain references tracked by runtime cycle collectors, traversal routines MUST query hashbrown::HashTable using safe iterator interfaces to register all reachable references.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute the test suite covering hash table data structures and runtime traversal.
# Run the project linting and formatting checks to verify adherence to codebase standards and type safety constraints.
```

**Accept when:**
- All unit and integration tests covering hash table operations, capacity growth, and reference traversal pass with zero failures.
- Static analysis and type checks report no errors or unresolved table invariants.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing unencapsulated low-level table operations or bypassing lock-file grounding are blocked from merging.
</enforcement>