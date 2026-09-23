# Adoption of std::borrow::Cow for Zero-Copy Data Handling in Serialization and Input Pipelines: Type Serializers Coordinating Shared Cross Thread

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-COW-001** MUST: Type serializers coordinating shared cross-thread or definitions state MUST combine std::borrow::Cow with thread-safe pointer structures rather than duplicating owned buffers across serializing routines.

### Verify

```bash
# Discover and run the project test suite covering type serializers
# Discover and run the project linter and static analysis checks
# Discover and execute benchmark suites to measure allocation behavior
```

**Accept when:**
- The project test suite passes completely without memory safety or lifetime violations.
- Static analysis and linting scripts report zero errors across serialization and input processing modules.
- Performance benchmarks confirm zero-copy paths avoid heap allocations during unescaped serialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites, static analysis checks, and peer code reviews verify compliance.
</enforcement>