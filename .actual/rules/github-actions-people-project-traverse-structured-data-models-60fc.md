# BaseModel Hierarchy Traversal for In-Memory Contributor Identity Aggregation: Project Traverse Structured Data Models Extending

These rules are ALWAYS ACTIVE for routines extracting participant identities from hierarchical external query payloads.

### Rules

- **R-TRAVERSE-001** MUST: Traverse structured data models extending BaseModel to extract identity attributes into deduplicated in-memory set collections when aggregating entity relationships.

### Verify

```bash
# Discover and run the project static analysis validation script to verify that attribute access on BaseModel subclasses conforms to declared optionality.
# Identify and execute the repository test runner from the project configuration to validate data access and identity aggregation routines.
```

**Accept when:**
- All data aggregation routines successfully extract identity fields via validated BaseModel instances without runtime attribute errors.
- The project test suite passes with complete test coverage over hierarchical data model traversal logic.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>