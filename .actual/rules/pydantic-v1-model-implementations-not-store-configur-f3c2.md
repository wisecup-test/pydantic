# Adoption of pydantic.v1.config for Model Configuration Architecture: Model Implementations Not Store Configuration State

These rules are ALWAYS ACTIVE for validation models, settings loaders, and argument decorators implemented within the core library namespace.

### Rules

- **R-PYD-001** MUST_NOT: Model implementations MUST_NOT store configuration state as instance attributes on the validated data structures.

### Verify

```bash
# Discover and run the repository test suite to verify model configuration behavior and validation source ordering
# Execute the repository static type verification workflow to ensure configuration inner classes satisfy base configuration type constraints
```

**Accept when:**
- All model and settings test suites pass with expected configuration behaviors and source resolution precedence verified.
- Static type analysis confirms inner configuration classes conform to BaseConfig specifications with zero type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory and enforced via automated CI pipelines, static analysis, and code review inspections.
</enforcement>