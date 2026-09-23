# Adoption of hashbrown::HashTable for Low-Level Hash Table Management: Internal Runtime Modules Requiring Direct Bucket

These rules are ALWAYS ACTIVE for internal runtime modules requiring direct bucket manipulation or specialized memory layouts.

### Rules

- **R-HT-001** MUST: Internal runtime modules requiring direct bucket manipulation or specialized memory layouts MUST adopt hashbrown::HashTable as the underlying low-level hash table data structure.

### Verify

```bash
# Discover and execute project test suite covering hash table data structures and runtime traversal
# Run project linting and formatting checks to verify adherence to codebase standards and type safety constraints
```

**Accept when:**
- All unit and integration tests covering hash table operations, capacity growth, and reference traversal pass with zero failures.
- Static analysis and type checks report no errors or unresolved table invariants.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test suites and mandatory peer code reviews enforce compliance.
</enforcement>