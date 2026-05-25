# Adopt Integration Testing Strategy with Core Metadata Validation: Teams Use Fixtures

These rules are ALWAYS ACTIVE for all integration testing implementations across the codebase. All test suites that validate component interactions MUST follow these integration testing patterns.

### Rules

- **R-INT-001** MAY: Teams MAY use fixtures or test harnesses to simplify integration test setup and reduce duplication across test files.

### Verify

```bash
# Check for pytest integration markers or dedicated integration test directories
grep -r "@pytest.mark.integration" tests/ || grep -r "test_integration" tests/

# Count integration test files
find . -path '*/tests/integration/*' -name 'test_*.py' | wc -l

# List all integration tests collected by pytest
pytest --collect-only -m integration 2>/dev/null | grep -c '<Module' || echo '0'
```

**Accept when:**
- Integration test markers or dedicated integration test directories are present in the codebase
- At least one integration test exists for each module in _internal/ that handles metadata or configuration
- CI pipeline includes a stage that runs integration tests separately from unit tests
- Test coverage reports show integration test coverage for cross-module interactions

<enforcement>
Claude Code MUST NOT skip or defer verification of integration test presence and organization. Violations identified in code review or CI must be escalated to the tech lead for exception handling or remediation.
</enforcement>