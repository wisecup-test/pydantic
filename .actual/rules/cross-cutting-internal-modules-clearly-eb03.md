# Standardize External Client Boundary Patterns in Internal APIs: Internal Modules Clearly

These rules are ALWAYS ACTIVE for all internal API modules that expose interfaces to external clients or require boundary validation, particularly in validation, configuration, and data transformation layers.

### Rules

- **R-BOUNDARY-001** MUST: Internal API modules MUST clearly separate external client interfaces from internal implementation details using explicit boundary markers (e.g., underscore prefixes, separate modules, or explicit exports).
- **R-BOUNDARY-002** MUST: Use Python's underscore prefix convention (_internal, _private) consistently to mark internal modules and functions not intended for external use.
- **R-BOUNDARY-003** MUST: Implement __all__ exports in public modules to explicitly control the external API surface.
- **R-BOUNDARY-004** SHOULD: Use type hints and protocols to define boundary contracts without exposing implementation details.
- **R-BOUNDARY-005** SHOULD: Document the boundary pattern in architecture guides and provide examples of correct usage for both internal and external developers.
- **R-BOUNDARY-006** SHOULD: Establish clear guidelines for when to create new boundary interfaces versus extending existing ones.

### Verify

```bash
# Check for external client code directly importing from _internal modules
grep -r 'from pydantic._internal' --include='*.py' --exclude-dir='pydantic' --exclude-dir='tests' . | wc -l | grep -q '^0$'

# Verify no _internal imports in external code using AST analysis
python -c "import ast; import sys; [sys.exit(1) for f in sys.argv[1:] if any('_internal' in n.module for n in ast.walk(ast.parse(open(f).read())) if isinstance(n, ast.ImportFrom) and n.module)]" $(find . -name '*.py' -not -path '*/pydantic/*' -not -path '*/tests/*')

# Check for internal modules missing __all__ exports
find . -path '*/_internal/*.py' -exec grep -L '^def\|^class' {} \; | xargs -r grep -L '__all__' | wc -l
```

**Accept when:**
- No external client code directly imports from _internal modules or uses underscore-prefixed APIs
- All public API modules explicitly define __all__ exports or have clear documentation of public interfaces
- Boundary validation is present in all external entry points to internal API functionality
- Code review process includes verification that new internal APIs follow boundary separation patterns
- Automated CI checks confirm no _internal module imports from external code
- Static analysis tools flag any direct access to internal APIs

<enforcement>
Claude Code MUST NOT skip or defer verification of boundary separation patterns. All rules in this file are mandatory for internal API implementations.
</enforcement>