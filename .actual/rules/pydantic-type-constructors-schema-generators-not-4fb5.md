# Adoption of DecoratorInfos for Class Decorator Extraction and Descriptor Proxy Resolution: Type Constructors Schema Generators Not Perform

These rules are ALWAYS ACTIVE for class construction metaclasses, dataclass transformation routines, and schema generation pipelines processing decorated data structures.

### Rules

- **R-ADR-001** MUST_NOT: Type constructors and schema generators MUST_NOT perform ad-hoc inspection of class attribute dictionaries or bypass DecoratorInfos when resolving descriptor proxies and wrapped methods.

### Verify

```bash
# Discover the project test runner configuration from the repository manifest and execute the test suite targeting decorator and model construction subsystems.
# Locate the linting and static analysis configurations in the project workspace and run the static verification checks across internal modules.
```

**Accept when:**
- All decorator extraction and model construction test suites execute without failures or unhandled descriptor warnings.
- Static type analysis confirms that all call sites conform to the builder interface parameters and expected return types.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration test suites, static type analysis, and mandatory architectural peer review.
</enforcement>