# Adoption of DecoratorInfos for Class Decorator Extraction and Descriptor Proxy Resolution: Decorator Metadata Collections Updated External Configuration

These rules are ALWAYS ACTIVE for class construction metaclasses, dataclass transformation routines, and schema generation pipelines processing decorated data structures.

### Rules

- **R-DEC-001** MAY: Decorator metadata collections MAY be updated with external configuration wrappers after initial construction to incorporate inheritance overrides.

### Verify

```bash
# Discover the project test runner configuration from the repository manifest and execute the test suite targeting decorator and model construction subsystems.
# Locate the linting and static analysis configurations in the project workspace and run the static verification checks across internal modules.
```

**Accept when:**
- All decorator extraction and model construction test suites execute without failures or unhandled descriptor warnings.
- Static type analysis confirms that all call sites conform to the builder interface parameters and expected return types.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated CI test suites, static analysis, and architectural peer reviews enforce compliance; non-compliant code will be blocked during review or CI failure.
</enforcement>