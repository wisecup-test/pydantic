# Pydantic BaseModel Schema Deserialization for External GraphQL API Protocols: Every External Graphql Client Invocation Explicitly

These rules are ALWAYS ACTIVE for routines that execute HTTP requests against external GraphQL endpoints, process external entity responses, and ingest data traversing nested discussion, comment, or contributor structures returned by third-party APIs.

### Rules

- **R-EXT-001** MUST: Every external GraphQL client invocation MUST explicitly evaluate HTTP response status codes and log top-level response error arrays before attempting schema deserialization.

### Verify

```bash
# Discover and execute the project verification script from the repository manifest covering external API client schemas
# Execute repository linting and static type checking directives defined in project configuration
```

**Accept when:**
- All unit and integration tests for external GraphQL schema deserialization pass with zero validation errors.
- Static type analysis confirms that all fields accessed in downstream processing routines align with declared model attributes.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new external API endpoints must declare corresponding BaseModel schemas.
</enforcement>