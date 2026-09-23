# BaseModel Hierarchy Traversal for In-Memory Contributor Identity Aggregation: Consumers Discover Ecosystem Lock File Verify

These rules are ALWAYS ACTIVE for routines extracting participant identities from hierarchical external query payloads.

### Rules

- **R-BASE-001** MUST: Consumers MUST discover the ecosystem lock file and verify the exact locked version of data modeling dependencies before implementing or modifying data model schemas.

### Verify

```bash
# Discover and run the project static analysis validation script to verify that attribute access on BaseModel subclasses conforms to declared optionality.
# Identify and execute the repository test runner from the project configuration to validate data access and identity aggregation routines.
```

**Accept when:**
- All data aggregation routines successfully extract identity fields via validated BaseModel instances without runtime attribute errors.
- The project test suite passes with complete test coverage over hierarchical data model traversal logic.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via automated continuous integration checks and peer code review.
</enforcement>