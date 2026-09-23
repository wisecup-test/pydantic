# BaseModel Domain Validation for External Response Ingestion: Nullable Fields Incoming Payloads Explicitly Typed

These rules are ALWAYS ACTIVE for ingestion and parsing of structured external API response payloads and domain entity modeling for external data structures passed to downstream business logic.

### Rules

- **R-EXT-001** SHOULD: Nullable fields in incoming payloads explicitly typed with optional unions to prevent schema validation failures on missing data.
- **R-EXT-002** MANDATORY: External payloads that contain arbitrary dynamic metadata that cannot be mapped to predefined structural schemas must be handled via explicit exception (EXC-56-001).

### Verify

```bash
# Discover and execute the project verification script from the repository manifest to run domain validation tests.
# Discover and execute the repository static analysis suite to verify model type annotations and boundary contracts.
```

**Accept when:**
- External data ingestion routines validate incoming structured payloads through declared domain models inheriting from BaseModel.
- Repository static analysis suites and test suites execute successfully without schema or type validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static type checking and continuous integration pipelines, and peer code reviews evaluating payload ingestion and domain modeling patterns.
</enforcement>