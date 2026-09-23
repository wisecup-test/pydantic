# BaseModel Domain Validation for External Response Ingestion: External Payload Ingestion Logic Parse Validate

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EXT-001** MUST: External API payload ingestion logic MUST parse and validate incoming structured data using domain model classes inheriting from BaseModel.

### Verify

```bash
# Discover and execute the project verification script from the repository manifest to run domain validation tests.
# Discover and execute the repository static analysis suite to verify model type annotations and boundary contracts.
```

**Accept when:**
- External data ingestion routines validate incoming structured payloads through declared domain models inheriting from BaseModel.
- Repository static analysis suites and test suites execute successfully without schema or type validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>