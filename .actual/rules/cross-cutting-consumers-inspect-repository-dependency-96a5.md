# Adoption of dataclasses Module for Structured Data Modeling and Field Introspection: Consumers Inspect Repository Dependency Manifest Lock

These rules are ALWAYS ACTIVE for internal modules responsible for schema gathering, field reflection, constructor signature generation, and data conversion/reporting utilities constructing structured table rows.

### Rules

- **R-DATACLASS-001** MUST: Consumers MUST inspect the repository dependency manifest and lock artifacts to determine resolved runtime versions before applying dataclass reflection APIs.

### Verify

```bash
# Discover the project test runner configuration from repository manifests and execute the test suite covering schema gathering, field extraction, and signature generation.
# Locate and run the repository static type analysis and linter scripts to verify dataclass usage and metadata typing compliance across internal modules.
```

**Accept when:**
- All automated test suites verifying dataclass field extraction, schema traversal, and signature generation execute successfully without errors.
- Static type analysis verifies that all field metadata structures and gathered schema references conform to defined type contracts without violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing unvalidated or non-standard dataclass introspection patterns fail continuous integration verification.
</enforcement>