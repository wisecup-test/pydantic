# Adopt Integration Testing Strategy with Core Metadata Validation: Modules That Handle

These rules are ALWAYS ACTIVE for all modules that handle core metadata, configuration, or cross-module interactions across the codebase.

### Rules

- **R-INT-001** MUST: All modules that handle core metadata, configuration, or cross-module interactions MUST include integration tests that validate behavior across component boundaries.
- **R-INT-002** MUST: Integration tests MUST use pytest markers (e.g., `@pytest.mark.integration`) to enable selective test execution and separate fast unit tests from slower integration tests.
- **R-INT-003** MUST: All _internal package modules that handle metadata or configuration MUST have corresponding integration tests.
- **R-INT-004** SHOULD: Create a `tests/integration` directory structure that mirrors the main codebase structure for easy navigation and maintenance.
- **R-INT-005** SHOULD: Develop reusable test fixtures for common integration scenarios (e.g., configuration setup, metadata initialization) to reduce boilerplate.
- **R-INT-006** SHOULD: Document integration test patterns and examples in the project's testing guide, including when to write integration vs unit tests.
- **R-INT-007** MAY: Consider using test containers or in-memory databases for integration tests to improve speed and reliability.

### Verify

```bash
# Check for integration test markers or dedicated integration test directories
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
Claude Code MUST NOT skip or defer verification of integration test presence and markers. Violations identified in code review or CI must be escalated to the tech lead for exception handling or remediation.
</enforcement>