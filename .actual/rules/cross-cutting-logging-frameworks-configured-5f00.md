# Standardize Structured Logging with Contextual Information: Logging Frameworks Configured

These rules are ALWAYS ACTIVE for all application code that generates log output, including backend services written in Python, frontend workers and JavaScript runtime environments, plugin systems and extension points, and error handling and exception logging paths.

### Rules

- **R-LOG-001** SHOULD: Logging frameworks SHOULD be configured consistently across language environments (Python, JavaScript) to maintain uniform log formats.

### Verify

```bash
# Check for consistent use of structured logging frameworks
grep -r "logger\." --include="*.py" --include="*.js" | grep -v "#" | head -20

# Count instances of anti-patterns (print/console.log)
grep -rE "(console\.log|print\()" --include="*.py" --include="*.js" | wc -l

# Identify logging calls in Python files
python -c "import ast; import sys; [print(f'{sys.argv[1]}:{node.lineno}') for node in ast.walk(ast.parse(open(sys.argv[1]).read())) if isinstance(node, ast.Call) and hasattr(node.func, 'attr') and node.func.attr in ['debug', 'info', 'warning', 'error', 'critical']]" docs/plugins/main.py
```

**Accept when:**
- Grep commands show consistent use of structured logging frameworks (logger.info, logger.error, etc.) rather than print statements or console.log
- Code review confirms that log statements include relevant contextual information (IDs, operation names, relevant state)
- Automated scans detect no instances of common sensitive data patterns (password=, api_key=, token=) in logging statements
- New code submissions include appropriate logging at key decision points and error handling paths

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated CI/CD pipeline checks scanning for logging anti-patterns and sensitive data exposure are mandatory. Code review checklist items specifically covering logging standards and context inclusion are required. Static analysis tools configured to flag use of print/console.log in production code paths must be applied.
</enforcement>