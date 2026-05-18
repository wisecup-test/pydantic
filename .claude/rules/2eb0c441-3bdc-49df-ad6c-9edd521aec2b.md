<rule_activation id="2eb0c441-3bdc-49df-ad6c-9edd521aec2b" title="Standardize Unit Testing Patterns for Public API Validation: Unit Tests Public" applies_to="**/*">
These rules are ALWAYS ACTIVE for all public/external API development and testing activities.
</rule_activation>

### Rules

- **R-API-001** MUST: Unit tests for public APIs MUST cover type validation, error handling, and edge cases including null/empty inputs

### Scope

**In scope:**
- All public API functions, methods, and classes exposed to external consumers
- Validator functions and validation logic accessible through public interfaces
- Data serialization/deserialization methods (JSON, dict conversion)
- Error types and error handling mechanisms in public APIs
- Type hints and typing utilities exposed as public API surface

**Out of scope:**
- Internal implementation details not exposed through public APIs
- Private helper functions and internal utilities
- Integration tests and end-to-end tests (covered by separate ADRs)
- Performance benchmarks and load testing
- Documentation examples (unless they serve as executable tests)

**Exceptions:**
- EXC-001: Deprecated API functions scheduled for removal in next major version
- EXC-002: Experimental APIs marked with explicit unstable/beta status

### Verify

```bash
# Run test suite with coverage analysis
pytest tests/ pydantic-core/tests/ -v --cov=. --cov-report=term-missing --cov-fail-under=90

# Count test files
find tests/ pydantic-core/tests/ -name 'test_*.py' | wc -l

# Count test functions
grep -r "def test_" tests/ pydantic-core/tests/ | wc -l
```

**Accept when:**
- All public API modules have corresponding test files with minimum 90% code coverage
- Test suite executes successfully with all tests passing in CI pipeline
- Each public API function/class has at least one positive test case and one negative/error test case
- Test files follow established naming conventions (test_<module>.py) and organizational structure

### Implementation Guidelines

- Organize test files to mirror the structure of the API module being tested (e.g., validators/test_dict.py tests validators/dict.py)
- Use descriptive test names that clearly indicate what contract or behavior is being validated (e.g., test_dict_validator_rejects_invalid_keys)
- Group related test cases using test classes or pytest markers to improve test organization and selective execution
- Include both positive test cases (valid inputs) and negative test cases (invalid inputs, edge cases, error conditions) for comprehensive coverage
- Leverage parametrized tests to efficiently cover multiple input variations without code duplication
- Document complex test scenarios with comments explaining the API contract being validated

<enforcement>
Claude Code MUST NOT skip or defer verification. All public API modules MUST meet the 90% code coverage threshold and pass all verification commands before changes are accepted.
</enforcement>