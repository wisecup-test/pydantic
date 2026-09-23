# Adoption of std::borrow::Cow for Zero-Copy Data Handling in Serialization and Input Pipelines: Core Serializer Implementations Input Processing Routines

These rules are ALWAYS ACTIVE for type serialization routines, input processing routines, and configuration mapping components handling string, sequence, or collection transformations.

### Rules

- **R-COW-001** MUST: The core serializer implementations and input processing routines MUST use std::borrow::Cow when handling string or sequence transformations where data can remain borrowed without allocation.
- **R-COW-002** EXCEPT: External interface contracts require an unconditionally owned string or buffer due to foreign function boundary memory ownership transference (EXC-20-001).

### Verify

```bash
# Discover the project manifest and execute the test suite covering type serializers
# Discover and run the project linter and static analysis checks
# Discover and execute benchmark suites to measure allocation behavior across serialization workloads
```

**Accept when:**
- The project test suite passes completely without memory safety or lifetime violations.
- Static analysis and linting scripts report zero errors across serialization and input processing modules.
- Performance benchmarks confirm zero-copy paths avoid heap allocations during unescaped serialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites, static analysis checks, and peer code reviews verify compliance.
</enforcement>