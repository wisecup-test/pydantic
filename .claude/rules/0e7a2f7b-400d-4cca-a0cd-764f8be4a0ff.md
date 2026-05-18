<rule_activation id="0e7a2f7b-400d-4cca-a0cd-764f8be4a0ff" title="Adopt pytest as Standard Testing Framework for Python Projects: Test Functions Prefixed" applies_to="tests/**/*.py">
These rules are ALWAYS ACTIVE for all Python test files in the tests/ directory.
</rule_activation>

### Rules

- **R-PYTEST-001** MUST: All test functions MUST be prefixed with 'test_' to enable automatic test discovery by pytest

### Verify

```bash
# Verify pytest is imported in test files
grep -r "^import pytest" tests/ || grep -r "^from pytest" tests/

# Verify test files follow naming conventions
find tests/ -name 'test_*.py' -o -name '*_test.py' | head -5

# Verify pytest can discover tests
pytest --collect-only tests/ 2>&1 | grep -E '(test session starts|collected)'

# Verify pytest is listed as a dependency
grep -E "pytest|pytest-cov" requirements.txt pyproject.toml setup.py setup.cfg 2>/dev/null || echo 'Check dependency files'
```

**Accept when:**
- pytest is listed as a dependency in project requirements files (requirements.txt, pyproject.toml, or setup.py)
- Test files follow pytest naming conventions (test_*.py) and are organized in tests/ directory
- pytest --collect-only successfully discovers test files without errors
- CI/CD pipeline configuration includes pytest execution commands
- At least 80% of test files import pytest or use pytest-specific features (fixtures, parametrize, marks)

<enforcement>
Claude Code MUST NOT skip or defer verification. All test functions in tests/ directory MUST be prefixed with 'test_' for pytest discovery compliance.
</enforcement>