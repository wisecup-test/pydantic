# BaseModel Domain Validation for External Response Ingestion: Engineers Discover Project Dependency Manifest Lock

These rules are ALWAYS ACTIVE for ingestion and parsing of structured external API response payloads and domain entity modeling for external data structures passed to downstream business logic.

### Rules

- **R-BASE-001** MUST: Engineers MUST discover the project dependency manifest and lock artifact to verify the exact resolved version of the data validation library before implementing model definitions.
- **R-BASE-002** MUST: Ingesting unvalidated external API response structures MUST use domain modeling with BaseModel to establish explicit schemas for entities during data ingestion boundaries.
- **R-BASE-003** MUST: Define domain model attributes to match external interface fields, restricting definitions to properties utilized by downstream routines.
- **R-BASE-004** MUST: Record diagnostic error output when external responses indicate communication failures or payload validation issues.

### Verify

```bash
# Discover and execute the project verification script from the repository manifest to run domain validation tests.
# Discover and execute the repository static analysis suite to verify model type annotations and boundary contracts.
```

**Accept when:**
- External data ingestion routines validate incoming structured payloads through declared domain models inheriting from BaseModel.
- Repository static analysis suites and test suites execute successfully without schema or type validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests that ingest external structured payloads without domain model validation are blocked pending schema definition.
</enforcement>