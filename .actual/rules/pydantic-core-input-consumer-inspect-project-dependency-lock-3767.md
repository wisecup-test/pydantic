# Adoption of IntoPyObject and Serialize Derive Traits for Data Modeling: Consumer Inspect Project Dependency Lock Artifact

These rules are ALWAYS ACTIVE for data modeling structures defining return values across language boundaries and domain models requiring automated serialization and language conversion.

### Rules

- **R-ADR-001** MUST: The consumer MUST inspect the project dependency lock artifact to resolve the exact locked versions of all external derivation dependencies before implementing trait derivations.

### Verify

```bash
# Discover and run project compilation and linting suite to ensure derive macros expand without errors
# Execute the test runner across data modeling test suites to validate serialization and conversion behaviors
```

**Accept when:**
- Compilation succeeds with all derive attributes cleanly expanding across targeted data structures.
- All serialization and conversion test assertions pass without regression across boundary contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration build checks and peer code reviews validate trait derivation compilation and macro adoption.
</enforcement>