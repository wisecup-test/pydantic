# BaseModel Settings Secret Encapsulation and Explicit get_secret_value Access: Application Configuration Models Holding Sensitive Credentials

These rules are ALWAYS ACTIVE for configuration schemas, settings definitions, and outbound client dispatch boundaries that ingest, manage, or transport sensitive authentication credentials.

### Rules

- **R-BS-001** MUST: Application configuration models holding sensitive credentials MUST encapsulate secret values within specialized secret attributes on BaseModel configurations rather than plain string fields.

### Verify

```bash
# Discover and run the project static analysis suite
# Discover and run the repository test suite to confirm configuration serialization masks credentials
```

**Accept when:**
- Configuration models serialize without revealing plaintext secrets when processed through model_dump_json.
- Secret credentials are retrieved exclusively through get_secret_value at external client dispatch boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>