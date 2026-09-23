# Adoption of DecoratorInfos for Class Decorator Extraction and Descriptor Proxy Resolution: When Class Level Decorators Alter Method

These rules are ALWAYS ACTIVE for class construction metaclasses, dataclass transformation routines, and schema generation pipelines processing decorated data structures.

### Rules

- **R-DEC-001** MUST: When class-level decorators alter method invocation semantics, `DecoratorInfos.build` MUST be invoked with wrapped method replacement enabled during class creation, and with replacement disabled when generating schemas for static type definitions.

### Verify

```bash
# Discover the project test runner configuration from the repository manifest and execute the test suite targeting decorator and model construction subsystems.
# Locate the linting and static analysis configurations in the project workspace and run the static verification checks across internal modules.
```

**Accept when:**
- All decorator extraction and model construction test suites execute without failures or unhandled descriptor warnings.
- Static type analysis confirms that all call sites conform to the builder interface parameters and expected return types.

<enforcement>
Claude Code MUST NOT skip or defer verification. All decorator extraction and model construction verification steps are mandatory.
</enforcement>