# Adoption of annotated_types for Canonical Type Constraint Metadata Representation: Modules Not Introduce Proprietary Duplicate Classes

These rules are ALWAYS ACTIVE for all code implementing field configuration metadata collection, constraint mapping, validation pipeline step composition, and custom scalar or temporal type constraint declarations.

### Rules

- **R-AT-001** MUST_NOT: Modules MUST NOT introduce proprietary duplicate classes for standard constraint definitions already provided by `annotated_types`.
- **R-AT-002** MANDATORY: The consumer MUST discover build tools, test runners, package managers, and versions from the project repository.
- **R-AT-003** MANDATORY: Prior to writing code using a versioned library, execute lock-version grounding by finding the manifest, identifying the build tool, inspecting lock/resolution artifacts, looking up official version-specific documentation, and confirming APIs exist.

### Verify

```bash
# Discover and execute the test runner covering field metadata extraction and constraint validation
# Discover the static analysis suite and verify type checking compliance across modules importing constraint metadata
```

**Accept when:**
- All field constraint arguments correctly instantiate and serialize into standard metadata descriptors.
- Pipeline constraint methods successfully construct execution chains using standard constraint descriptors without runtime type errors.
- Introspection and annotation reconstruction suites pass without regressions across all field configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated test suites, static type analysis, and peer code review is mandatory.
</enforcement>