# Adoption of pydantic.v1.config for Model Configuration Architecture: Dynamic Name Resolution Environment Key Mappings

These rules are ALWAYS ACTIVE for validation models, settings loaders, and argument decorators implemented within the core library namespace requiring custom source resolution or strict field validation policies.

### Rules

- **R-PYD-001** SHOULD: Dynamic name resolution and environment key mappings SHOULD be resolved through prepare_field on the model configuration class rather than at instantiation time.
- **R-PYD-002** MANDATORY: Ensure custom model implementations subclass BaseConfig when declaring inner configuration classes to guarantee standard defaults and hook compatibility.
- **R-PYD-003** MANDATORY: Utilize classmethod hooks within configuration classes to modify source priorities or parse environment variables rather than mutating global module state.

### Verify

```bash
# Discover and run the repository test suite to verify model configuration behavior and validation source ordering
pytest
# Execute the repository static type verification workflow to ensure configuration inner classes satisfy base configuration type constraints
mypy
```

**Accept when:**
- All model and settings test suites pass with expected configuration behaviors and source resolution precedence verified.
- Static type analysis confirms inner configuration classes conform to BaseConfig specifications with zero type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All model implementations must define inner configuration classes adhering to BaseConfig and utilize prepare_field for dynamic name resolution.
</enforcement>