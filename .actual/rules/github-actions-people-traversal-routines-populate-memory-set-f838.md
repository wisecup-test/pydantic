# BaseModel Hierarchy Traversal for In-Memory Contributor Identity Aggregation: Traversal Routines Populate Memory Set Collections

These rules are ALWAYS ACTIVE for all routines extracting participant identities from hierarchical external query payloads.

### Rules

- **R-TRAV-001** SHOULD: Traversal routines populate in-memory set collections directly during node iteration to guarantee unique identity extraction.
- **R-TRAV-002** MANDATORY: Discover and use the project lock-version grounding sequence before writing code that uses a versioned library.
- **R-TRAV-003** MANDATORY: Model classes representing comment and discussion nodes must declare author fields as optional to accommodate deleted users or system-generated messages.
- **R-TRAV-004** MANDATORY: Deduplication sets must be instantiated at the start of traversal routines and populated using set addition operations during node iteration.

### Verify

```bash
# Discover and run the project static analysis validation script to verify attribute access on BaseModel subclasses
# Identify and execute the repository test runner to validate data access and identity aggregation routines
```

**Accept when:**
- All data aggregation routines successfully extract identity fields via validated BaseModel instances without runtime attribute errors.
- The project test suite passes with complete test coverage over hierarchical data model traversal logic.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>