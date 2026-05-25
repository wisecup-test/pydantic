# Standardize External Client Boundary Patterns in Internal APIs: Configuration Metadata Handling

These rules are ALWAYS ACTIVE for all internal API implementations that expose interfaces to external clients or require boundary validation.

### Rules

- **R-BOUNDARY-001** SHOULD: Configuration and metadata handling for external clients SHOULD be isolated in dedicated boundary modules to enable independent evolution.

### Verify

```bash
# Check that no external client code directly imports from _internal modules
grep -r 'from pydantic._internal' --include='*.py' --exclude-dir='pydantic' --exclude-dir='tests' . | wc -l | grep -q '^0$'

# Verify no _internal imports in external code using AST analysis
python -c "import ast; import sys; [sys.exit(1) for f in sys.argv[1:] if any('_internal' in n.module for n in ast.walk(ast.parse(open(f).read())) if isinstance(n, ast.ImportFrom) and n.module)]" $(find . -name '*.py' -not -path '*/pydantic/*' -not -path '*/tests/*')

# Check for internal modules lacking __all__ exports
find . -path '*/_internal/*.py' -exec grep -L '^def\|^class' {} \; | xargs -r grep -L '__all__' | wc -l
```

**Accept when:**
- No external client code directly imports from _internal modules or uses underscore-prefixed APIs
- All public API modules explicitly define __all__ exports or have clear documentation of public interfaces
- Boundary validation is present in all external entry points to internal API functionality
- Code review process includes verification that new internal APIs follow boundary separation patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. All verify commands MUST pass before accepting changes to internal API boundary implementations.
</enforcement>