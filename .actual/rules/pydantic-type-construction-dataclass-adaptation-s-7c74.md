# Adoption of DecoratorInfos for Class Decorator Extraction and Descriptor Proxy Resolution: Type Construction Dataclass Adaptation Schema Generation

These rules are ALWAYS ACTIVE for all type construction, dataclass adaptation, and schema generation routines.

### Rules

- **R-DEC-001** MUST: All type construction, dataclass adaptation, and schema generation routines MUST utilize DecoratorInfos.build to extract, validate, and collect decorator metadata from target class namespaces.

### Verify

```bash
# Discover the project test runner configuration from the repository manifest and execute the test suite targeting decorator and model construction subsystems.
# Locate the linting and static analysis configurations in the project workspace and run the static verification checks across internal modules.
```

**Accept when:**
- All decorator extraction and model construction test suites execute without failures or unhandled descriptor warnings.
- Static type analysis confirms that all call sites conform to the builder interface parameters and expected return types.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration test suites, static type analysis, linting checks, and mandatory architectural peer review.
</enforcement>