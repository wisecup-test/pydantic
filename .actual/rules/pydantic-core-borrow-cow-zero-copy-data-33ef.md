# Adoption of std::borrow::Cow for Zero-Copy Data Handling in Serialization and Input Pipelines: Modules Handling Serialization Configuration Schema Errors

These rules are ALWAYS ACTIVE for all modules handling serialization configuration and schema errors, type serialization routines, and input processing routines.

### Rules

- **R-COW-001** MUST: Modules handling serialization configuration and schema errors MUST allocate Cow::Owned variants only when string mutation, value encoding, or runtime generation requires taking ownership.

### Verify

```bash
# Discover and execute the test runner covering type serializers from project manifest
# Discover and run the project linter and static analysis checks
# Discover and execute benchmark suites to measure allocation behavior
```

**Accept when:**
- The project test suite passes completely without memory safety or lifetime violations.
- Static analysis and linting scripts report zero errors across serialization and input processing modules.
- Performance benchmarks confirm zero-copy paths avoid heap allocations during unescaped serialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites, peer review, and static analysis checks are mandatory.
</enforcement>