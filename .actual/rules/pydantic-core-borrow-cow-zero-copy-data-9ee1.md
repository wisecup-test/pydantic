# Adoption of std::borrow::Cow for Zero-Copy Data Handling in Serialization and Input Pipelines: Consumers Inspect Repository Dependency Manifest Corresponding

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-COW-001** MUST: Consumers MUST inspect the repository dependency manifest and corresponding lock artifact to verify compiler and library compatibility before implementing or modifying routines using borrowed container patterns.

### Verify

```bash
# Discover and execute project test suite covering type serializers
# Discover and run project linter and static analysis checks
# Discover and execute benchmark suites
```

**Accept when:**
- The project test suite passes completely without memory safety or lifetime violations.
- Static analysis and linting scripts report zero errors across serialization and input processing modules.
- Performance benchmarks confirm zero-copy paths avoid heap allocations during unescaped serialization.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>