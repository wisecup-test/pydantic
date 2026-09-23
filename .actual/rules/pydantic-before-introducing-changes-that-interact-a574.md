# Adoption of DecoratorInfos for Class Decorator Extraction and Descriptor Proxy Resolution: Before Introducing Changes That Interact Versioned

These rules are ALWAYS ACTIVE for class construction metaclasses, dataclass transformation routines, and schema generation pipelines processing decorated data structures.

### Rules

- **R-DEC-001** MUST: Before introducing changes that interact with versioned dependencies or runtime execution environments, the consumer MUST inspect the repository lock artifact to determine the exact resolved versions and verify interface compatibility against official release documentation.
- **R-DEC-002** MUST: Centralize decorator extraction via the builder interface to guarantee that inheritance traversal, descriptor unwrapping, and metadata indexing occur identically across all supported class structures.
- **R-DEC-003** MUST: Enforce boolean parameter flags that explicitly control method replacement during decorator extraction, ensuring active wrapping during metaclass execution and prohibiting method replacement in read-only schema generation pathways.

### Verify

```bash
# Discover the project test runner configuration from the repository manifest and execute the test suite targeting decorator and model construction subsystems.
# Locate the linting and static analysis configurations in the project workspace and run the static verification checks across internal modules.
```

**Accept when:**
- All decorator extraction and model construction test suites execute without failures or unhandled descriptor warnings.
- Static type analysis confirms that all call sites conform to the builder interface parameters and expected return types.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration test suites, static type analysis, and mandatory architectural peer review of changes to class construction and decorator compilation modules.
</enforcement>