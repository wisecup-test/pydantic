# BaseModel Settings Secret Encapsulation and Explicit get_secret_value Access: Logging Statements Debugging Routines Not Log

These rules are ALWAYS ACTIVE for all configuration schemas, settings definitions, and client dispatch boundaries that ingest, manage, or transport authentication credentials.

### Rules

- **R-BASE-001** MUST_NOT: Logging statements and debugging routines MUST_NOT log the output of get_secret_value or bypass serialization masking.

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