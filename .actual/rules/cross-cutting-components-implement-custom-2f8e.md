# Standardize Structured Logging with Contextual Information: Components Implement Custom

These rules are ALWAYS ACTIVE for all application code that generates log output, including backend services written in Python, frontend workers and JavaScript runtime environments, plugin systems and extension points, and error handling and exception logging paths.

### Rules

- **R-LOG-001** MAY: Components MAY implement custom logging adapters or wrappers to add component-specific context automatically.

### Verify

```bash
# Check for consistent use of structured logging frameworks
grep -r "logger\." --include="*.py" --include="*.js" | grep -v "#" | head -20

# Count use of print statements and console.log (should be minimal in production code)
grep -rE "(console\.log|print\()" --include="*.py" --include="*.js" | wc -l

# Find logging calls in Python files with line numbers
python -c "import ast; import sys; [print(f'{sys.argv[1]}:{node.lineno}') for node in ast.walk(ast.parse(open(sys.argv[1]).read())) if isinstance(node, ast.Call) and hasattr(node.func, 'attr') and node.func.attr in ['debug', 'info', 'warning', 'error', 'critical']]" docs/plugins/main.py
```

**Accept when:**
- Grep commands show consistent use of structured logging frameworks (logger.info, logger.error, etc.) rather than print statements or console.log
- Code review confirms that log statements include relevant contextual information (IDs, operation names, relevant state)
- Automated scans detect no instances of common sensitive data patterns (password=, api_key=, token=) in logging statements
- New code submissions include appropriate logging at key decision points and error handling paths

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations of sensitive data logging rules block merge in CI/CD. Missing context or inappropriate log levels require code review revision before approval.
</enforcement>