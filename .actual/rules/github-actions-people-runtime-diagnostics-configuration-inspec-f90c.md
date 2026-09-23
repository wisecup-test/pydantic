# BaseModel Settings Secret Encapsulation and Explicit get_secret_value Access: Runtime Diagnostics Configuration Inspection Serialize Settings

These rules are ALWAYS ACTIVE for configuration schemas, settings definitions, and outbound client dispatch boundaries that ingest, manage, or transport authentication credentials.

### Rules

- **R-DIAG-001** SHOULD: Runtime diagnostics and configuration inspection SHOULD serialize settings exclusively via model_dump_json to guarantee automated secret masking across all log sinks.
- **R-DIAG-002** MANDATORY: Define all credential properties on BaseModel settings structures using specialized secret string types to ensure automatic redaction during serialization.
- **R-DIAG-003** MANDATORY: Isolate calls to get_secret_value to the construction of external authorization headers immediately preceding external request execution.

### Verify

```bash
# Discover and run the project static analysis suite to verify secrets are not exposed in plaintext string fields
# Discover and run the repository test suite to confirm configuration serialization masks credentials under model_dump_json
```

**Accept when:**
- Configuration models serialize without revealing plaintext secrets when processed through model_dump_json.
- Secret credentials are retrieved exclusively through get_secret_value at external client dispatch boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis and peer code review.
</enforcement>