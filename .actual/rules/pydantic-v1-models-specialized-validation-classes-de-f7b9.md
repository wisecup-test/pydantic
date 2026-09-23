# Adoption of pydantic.v1.config for Model Configuration Architecture: Models Specialized Validation Classes Declare Execution

These rules are ALWAYS ACTIVE for all validation models, settings loaders, and function argument decorators implemented within the core library namespace.

### Rules

- **R-MOD-001** MUST: Models and specialized validation classes MUST declare execution rules and ingestion behaviors through an inner Config class inheriting from BaseConfig defined in pydantic.v1.config.

### Verify

```bash
# Discover and run the repository test suite to verify model configuration behavior and validation source ordering
# Execute the repository static type verification workflow to ensure configuration inner classes satisfy base configuration type constraints
```

**Accept when:**
- All model and settings test suites pass with expected configuration behaviors and source resolution precedence verified.
- Static type analysis confirms inner configuration classes conform to BaseConfig specifications with zero type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>