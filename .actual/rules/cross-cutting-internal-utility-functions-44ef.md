# Standardize External Client Boundary Patterns in Internal APIs: Internal Utility Functions

These rules are ALWAYS ACTIVE for all internal API implementations that expose interfaces to external clients or require boundary validation.

### Rules

- **R-BOUNDARY-001** MUST: Internal utility functions and classes that are not intended for external use MUST be prefixed with underscore or placed in _internal modules

### Verify

```bash
# Check for external client code directly importing from _internal modules
grep -r 'from pydantic._internal' --include='*.py' --exclude-dir='pydantic' --exclude-dir='tests' . | wc -l | grep -q '^0$'

# Verify no _internal imports in external code using AST analysis
python -c "import ast; import sys; [sys.exit(1) for f in sys.argv[1:] if any('_internal' in n.module for n in ast.walk(ast.parse(open(f).read())) if isinstance(n, ast.ImportFrom) and n.module)]" $(find . -name '*.py' -not -path '*/pydantic/*' -not -path '*/tests/*')

# Check that _internal modules define __all__ or contain definitions
find . -path '*/_internal/*.py' -exec grep -L '^def\|^class' {} \; | xargs -r grep -L '__all__' | wc -l
```

**Accept when:**
- No external client code directly imports from _internal modules or uses underscore-prefixed APIs
- All public API modules explicitly define __all__ exports or have clear documentation of public interfaces
- Boundary validation is present in all external entry points to internal API functionality
- Code review process includes verification that new internal APIs follow boundary separation patterns

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. Automated CI checks MUST scan for imports of _internal modules from external code. Code review MUST include checklist items verifying boundary pattern compliance. Violations result in CI pipeline failure and code review blocks until remediated or documented exceptions are approved.
</enforcement>