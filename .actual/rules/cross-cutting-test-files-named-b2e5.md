# Adopt pytest as Standard Testing Framework: Test Files Named

These rules are ALWAYS ACTIVE for all Python test files in the project, including unit tests for validators and serializers, integration tests for core functionality, test utilities and helper modules, and continuous integration test execution.

### Rules

- **R-PYTEST-001** MUST: Test files MUST be named with the `test_` prefix and placed in dedicated test directories.

### Verify

```bash
# Count pytest imports in test files
grep -r "^import pytest" tests/ pydantic-core/tests/ | wc -l

# Count test files following naming convention
find tests/ pydantic-core/tests/ -name 'test_*.py' -type f | wc -l

# Verify pytest can discover and collect all tests
pytest --collect-only -q 2>&1 | grep -E '^[0-9]+ tests? collected'
```

**Accept when:**
- All test files in `tests/` and `pydantic-core/tests/` directories follow `test_*.py` naming convention
- pytest successfully discovers and collects all test functions without errors
- No `unittest.TestCase` class-based tests exist in the codebase (or documented exceptions only)
- CI pipeline runs pytest and passes all tests

<enforcement>
Claude Code MUST NOT skip or defer verification. All test files must follow the `test_*.py` naming convention and be discoverable by pytest. Violations must be caught during code review and CI pipeline execution.
</enforcement>