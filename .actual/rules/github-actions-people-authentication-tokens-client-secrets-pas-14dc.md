# Pydantic BaseModel Schema Deserialization for External GraphQL API Protocols: Authentication Tokens Client Secrets Passed External

These rules are ALWAYS ACTIVE for routines that execute HTTP requests against external GraphQL endpoints, process external entity responses, and handle authentication tokens or client secrets.

### Rules

- **R-EXT-001** MUST: Authentication tokens and client secrets passed to external API endpoints MUST be retrieved using secret masking accessors rather than raw environment string exposure.

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