# Pydantic BaseModel Schema Deserialization for External GraphQL API Protocols: Before Implementing Updating Any Schema Models

These rules are ALWAYS ACTIVE for routines that execute HTTP requests against external GraphQL endpoints, process external entity responses, and data ingestion workflows that traverse nested discussion, comment, or contributor structures returned by third-party APIs.

### Rules

- **R-PYD-001** MUST: Before implementing or updating any schema models that depend on an external serialization library, the developer MUST inspect the authoritative lock artifact in the repository to resolve the exact dependency version and verify API compatibility.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute the test suite covering external API client schemas.
# Execute the repository linting and static type checking directives defined in the project configuration to confirm schema conformance.
```

**Accept when:**
- All unit and integration tests for external GraphQL schema deserialization pass with zero validation errors.
- Static type analysis confirms that all fields accessed in downstream processing routines align with declared model attributes.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>