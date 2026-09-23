# Adoption of std::borrow::Cow for Zero-Copy Data Handling in Serialization and Input Pipelines: Serializer Methods Returning Textual Formatted Outputs

These rules are ALWAYS ACTIVE for type serialization routines, input processing routines, and configuration mapping components handling dynamically encoded outputs.

### Rules

- **R-SERIALIZER-001** MUST: Serializer methods returning textual or formatted outputs MUST return Cow::Borrowed when the underlying representation can be directly referenced from the source input.

### Verify

```bash
# 1. Find the dependency manifest in the repo and identify build tool / test runner
# 2. Discover the project test runner and execute the test suite covering type serializers
# 3. Discover and run the project linter and static analysis checks to ensure compliance with lifetime and memory management guidelines
# 4. Discover and execute benchmark suites to measure allocation behavior across serialization workloads
```

**Accept when:**
- The project test suite passes completely without memory safety or lifetime violations.
- Static analysis and linting scripts report zero errors across serialization and input processing modules.
- Performance benchmarks confirm zero-copy paths avoid heap allocations during unescaped serialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites and static analysis checks are mandatory.
</enforcement>