# BaseModel Settings Secret Encapsulation and Explicit get_secret_value Access: Components Requiring Secret Credentials Access Underlying

These rules are ALWAYS ACTIVE for all configuration schemas, settings definitions, and outbound client dispatch boundaries that ingest, manage, or transport authentication credentials.

### Rules

- **R-BS-001** MUST: Components requiring secret credentials MUST access the underlying plain-text value strictly through explicit get_secret_value calls at the external communication boundary.

### Verify

```bash
# Discover and run the project static analysis suite to verify that secrets are not exposed in plaintext string fields.
# Discover and run the repository test suite to confirm configuration serialization masks credentials under model_dump_json.
```

**Accept when:**
- Configuration models serialize without revealing plaintext secrets when processed through model_dump_json.
- Secret credentials are retrieved exclusively through get_secret_value at external client dispatch boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>