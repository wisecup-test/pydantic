# Adoption of pydantic.v1.config for Model Configuration Architecture: Specialized Validation Constructs Configure Field Tolerance

These rules are ALWAYS ACTIVE for specialized data validation components, environment settings loaders, and function argument decorators implemented within the core library namespace.

### Rules

- **R-PVD-001** MUST: Specialized validation constructs MUST configure field tolerance explicitly using Extra definitions provided by pydantic.v1.config to prevent attribute injection.

### Verify

```bash
# Discover and run the repository test suite to verify model configuration behavior and validation source ordering.
# Execute the repository static type verification workflow to ensure configuration inner classes satisfy base configuration type constraints.
```

**Accept when:**
- All model and settings test suites pass with expected configuration behaviors and source resolution precedence verified.
- Static type analysis confirms inner configuration classes conform to BaseConfig specifications with zero type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>