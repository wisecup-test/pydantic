# Adoption of IntoPyObject and Serialize Derive Traits for Data Modeling: Data Modeling Structures That Serve Boundary

These rules are ALWAYS ACTIVE for all data modeling structures defining return values across language boundaries and domain models requiring automated serialization and language conversion.

### Rules

- **R-ADR-001** MUST: Data modeling structures that serve boundary return contracts MUST derive Serialize and IntoPyObject traits to ensure uniform cross-boundary representation.

### Verify

```bash
# Discover and run the project compilation and linting suite to ensure derive macros expand without errors.
# Execute the test runner across data modeling test suites to validate serialization and conversion behaviors.
```

**Accept when:**
- Compilation succeeds with all derive attributes cleanly expanding across targeted data structures.
- All serialization and conversion test assertions pass without regression across boundary contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration build checks validating trait derivation compilation and peer code review.
</enforcement>