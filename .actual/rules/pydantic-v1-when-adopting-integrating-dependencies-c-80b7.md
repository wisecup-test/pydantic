# Adoption of pydantic.v1.config for Model Configuration Architecture: When Adopting Integrating Dependencies Consumers Inspect

These rules are ALWAYS ACTIVE for validation models, settings loaders, and argument decorators implemented within the core library namespace.

### Rules

- **R-ADR-001** MUST: When adopting or integrating dependencies, consumers MUST inspect the repository lock artifact to verify the exact resolved dependency version prior to configuring model behaviors.

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