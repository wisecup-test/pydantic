# Pydantic BaseModel Schema Deserialization for External GraphQL API Protocols: Downstream Domain Aggregation Routines Not Access

These rules are ALWAYS ACTIVE for routines that execute HTTP requests against external GraphQL endpoints and process external entity responses, including data ingestion workflows that traverse nested discussion, comment, or contributor structures returned by third-party APIs.

### Rules

- **R-PDS-001** MUST_NOT: Downstream domain aggregation routines MUST NOT access raw response dictionaries directly when structured BaseModel schemas are defined for the corresponding API protocol.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute the test suite covering external API client schemas.
# Execute the repository linting and static type checking directives defined in the project configuration to confirm schema conformance.
```

**Accept when:**
- All unit and integration tests for external GraphQL schema deserialization pass with zero validation errors.
- Static type analysis confirms that all fields accessed in downstream processing routines align with declared model attributes.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing untyped dictionary access to external API responses will be blocked until schema models are introduced.
</enforcement>