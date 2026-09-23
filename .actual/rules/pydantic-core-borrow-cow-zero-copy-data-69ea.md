# Adoption of std::borrow::Cow for Zero-Copy Data Handling in Serialization and Input Pipelines: Serializer Implementations Not Unconditionally Clone Incoming

These rules are ALWAYS ACTIVE for all files matching type serialization routines processing string, formatted, or collection values, input processing routines extracting and returning enum, string, or boolean representations, and configuration mapping and serialization state components handling dynamically encoded outputs.

### Rules

- **R-SER-001** MUST_NOT: Serializer implementations MUST NOT unconditionally clone incoming string or slice data when the lifetime of the input argument permits a borrowed reference.

### Verify

```bash
# Discover the repository test runner from the project manifest and execute the test suite covering type serializers.
# Discover and run the project linter and static analysis checks to ensure compliance with lifetime and memory management guidelines.
# Discover and execute benchmark suites to measure allocation behavior across serialization workloads.
```

**Accept when:**
- The project test suite passes completely without memory safety or lifetime violations.
- Static analysis and linting scripts report zero errors across serialization and input processing modules.
- Performance benchmarks confirm zero-copy paths avoid heap allocations during unescaped serialization.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>