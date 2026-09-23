# Adoption of pydantic.v1.config for Model Configuration Architecture: Multi Source Configuration Loaders Inheriting Settings

These rules are ALWAYS ACTIVE for validation models, settings loaders, and argument decorators implemented within the core library namespace that require custom source resolution or strict field validation policies.

### Rules

- **R-PYD-001** SHOULD: Multi-source configuration loaders inheriting from settings abstractions MUST implement customise_sources on the configuration class to establish deterministic precedence across sources.

### Verify

```bash
# Discover and run the repository test suite to verify model configuration behavior and validation source ordering.
# Execute the repository static type verification workflow to ensure configuration inner classes satisfy base configuration type constraints.
```

**Accept when:**
- All model and settings test suites pass with expected configuration behaviors and source resolution precedence verified.
- Static type analysis confirms inner configuration classes conform to BaseConfig specifications with zero type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated CI test pipelines and code review inspections.
</enforcement>