# Adoption of dataclasses Module for Structured Data Modeling and Field Introspection: Core Schema Traversal Routines Not Bypass

These rules are ALWAYS ACTIVE for internal modules responsible for schema gathering, field reflection, and constructor signature generation, as well as data conversion and reporting utilities constructing structured table rows.

### Rules

- **R-SCHEMA-001** MUST_NOT: Core schema traversal routines MUST_NOT bypass definition reference mapping when collecting schema metadata from dataclass models.

### Verify

```bash
# Discover the project test runner configuration from repository manifests and execute the test suite covering schema gathering, field extraction, and signature generation.
# Locate and run the repository static type analysis and linter scripts to verify dataclass usage and metadata typing compliance across internal modules.
```

**Accept when:**
- All automated test suites verifying dataclass field extraction, schema traversal, and signature generation execute successfully without errors.
- Static type analysis verifies that all field metadata structures and gathered schema references conform to defined type contracts without violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated continuous integration test suites, static type checking, and peer code review is mandatory. Pull requests introducing unvalidated or non-standard dataclass introspection patterns fail continuous integration verification.
</enforcement>