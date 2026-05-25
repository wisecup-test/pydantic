# Standardize External Client Boundary Patterns in Internal APIs: Internal Expose Advanced

These rules are ALWAYS ACTIVE for all internal API implementations that expose interfaces to external clients or require boundary validation.

### Rules

- **R-EX-001** MAY: Internal APIs MAY expose advanced interfaces for power users through explicitly documented extension points with stability warnings.

### Verify

```bash
# Check for external client code directly importing from _internal modules
grep -r 'from pydantic._internal' --include='*.py' --exclude-dir='pydantic' --exclude-dir='tests' . | wc -l | grep -q '^0$'

# Verify no _internal imports in external code using AST analysis
python -c "import ast; import sys; [sys.exit(1) for f in sys.argv[1:] if any('_internal' in n.module for n in ast.walk(ast.parse(open(f).read())) if isinstance(n, ast.ImportFrom) and n.module)]" $(find . -name '*.py' -not -path '*/pydantic/*' -not -path '*/tests/*')

# Check that internal modules define __all__ exports
find . -path '*/_internal/*.py' -exec grep -L '^def\|^class' {} \; | xargs -r grep -L '__all__' | wc -l
```

**Accept when:**
- No external client code directly imports from _internal modules or uses underscore-prefixed APIs
- All public API modules explicitly define __all__ exports or have clear documentation of public interfaces
- Boundary validation is present in all external entry points to internal API functionality
- Code review process includes verification that new internal APIs follow boundary separation patterns
- Advanced interfaces for power users are explicitly documented with stability warnings
- Extension points are clearly marked and their stability guarantees are communicated

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and CI pipeline enforcement.
</enforcement>