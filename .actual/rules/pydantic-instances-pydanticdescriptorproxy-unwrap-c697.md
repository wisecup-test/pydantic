# Adoption of DecoratorInfos for Class Decorator Extraction and Descriptor Proxy Resolution: Instances Pydanticdescriptorproxy Unwrapped Retrieve Underlying Callables

These rules are ALWAYS ACTIVE for class construction metaclasses, dataclass transformation routines, and schema generation pipelines processing decorated data structures.

### Rules

- **R-DEC-001** SHOULD: Instances of PydanticDescriptorProxy MUST be unwrapped to retrieve underlying callables while preserving descriptor protocols across attribute lookups.

### Verify

```bash
# Discover project test runner from manifest and execute test suite for decorator/model construction
# Locate linting/static analysis configs and run checks across internal modules
```

**Accept when:**
- All decorator extraction and model construction test suites execute without failures or unhandled descriptor warnings.
- Static type analysis confirms that all call sites conform to the builder interface parameters and expected return types.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes to class construction and decorator compilation modules are subject to mandatory architectural peer review, static type analysis, and automated CI test execution.
</enforcement>