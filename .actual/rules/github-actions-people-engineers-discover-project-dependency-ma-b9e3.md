# BaseModel Settings Secret Encapsulation and Explicit get_secret_value Access: Engineers Discover Project Dependency Manifest Lock

These rules are ALWAYS ACTIVE for configuration schemas, settings definitions, and outbound client dispatch boundaries that ingest, manage, or transport authentication credentials.

### Rules

- **R-SEC-001** MUST: Engineers MUST discover the project dependency manifest and lock artifact to verify the exact resolved version of the data validation library before implementing or upgrading schema definitions.
- **R-SEC-002** MUST: Define all credential properties on BaseModel settings structures using specialized secret string types to ensure automatic redaction during serialization.
- **R-SEC-003** MUST: Isolate calls to get_secret_value to the construction of external authorization headers immediately preceding external request execution.

### Verify

```bash
# Discover and run the project static analysis suite to verify that secrets are not exposed in plaintext string fields.
# Discover and run the repository test suite to confirm configuration serialization masks credentials under model_dump_json.
```

**Accept when:**
- Configuration models serialize without revealing plaintext secrets when processed through model_dump_json.
- Secret credentials are retrieved exclusively through get_secret_value at external client dispatch boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and peer code reviews enforce compliance.
</enforcement>