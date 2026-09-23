# BaseModel Hierarchy Traversal for In-Memory Contributor Identity Aggregation: Data Access Routines Author Identity Attributes

These rules are ALWAYS ACTIVE for routines extracting participant identities from hierarchical external query payloads.

### Rules

- **R-BASE-001** MUST: Data access routines MUST access author identity attributes exclusively through validated model properties rather than performing dynamic dictionary subscripting on raw payload responses.

### Verify

```bash
# Discover and run the project static analysis validation script to verify that attribute access on BaseModel subclasses conforms to declared optionality.
# Identify and execute the repository test runner from the project configuration to validate data access and identity aggregation routines.
```

**Accept when:**
- All data aggregation routines successfully extract identity fields via validated BaseModel instances without runtime attribute errors.
- The project test suite passes with complete test coverage over hierarchical data model traversal logic.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing unvalidated dictionary access or direct payload indexing will be blocked during review.
</enforcement>