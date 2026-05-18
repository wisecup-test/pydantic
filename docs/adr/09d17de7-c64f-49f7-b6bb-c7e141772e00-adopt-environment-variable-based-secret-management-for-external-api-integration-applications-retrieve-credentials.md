# Adopt Environment Variable-Based Secret Management for External API Integration: Applications Retrieve Credentials

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- The codebase integrates with multiple external APIs (GitHub, Algolia, and other third-party services) that require authentication credentials
- Sensitive API keys and tokens must be managed securely to prevent exposure in version control systems and public repositories
- The pattern was detected across 6 files with 90.70% confidence, indicating consistent adoption of environment variable-based secrets management
- The facet 'security.secrets_management' suggests this pattern specifically addresses secure handling of authentication credentials for external API integrations
- Files include pydantic type definitions, GitHub actions, documentation plugins, and JSON schema handlers, all requiring secure API access

## Problem Statement

External API integrations require authentication credentials that must be kept secure and separate from application code. Hardcoding secrets in source files creates security vulnerabilities, makes credential rotation difficult, and violates security best practices. A standardized approach is needed to manage API keys, tokens, and other sensitive credentials across different environments while maintaining security and operational flexibility.

## Decision

1. MUST: Applications MUST retrieve API credentials from environment variables at runtime using standard environment variable access patterns

## Policy Block

- MUST Applications MUST retrieve API credentials from environment variables at runtime using standard environment variable access patterns

In scope:
- All external API integrations requiring authentication (GitHub API, Algolia, third-party services)
- API keys, tokens, OAuth credentials, and other authentication secrets
- Development, staging, and production environments
- CI/CD pipelines and GitHub Actions workflows
- Documentation build processes requiring external API access

Out of scope:
- Internal service-to-service communication within the same security boundary
- Public API endpoints that do not require authentication
- Configuration values that are not sensitive (e.g., API base URLs, timeout values)
- Test fixtures using mock credentials in isolated test environments

Exceptions:
- EXC-001: Unit tests may use hardcoded mock credentials when testing against mock API servers with no external connectivity

## Rationale

- Environment variable-based secret management is an industry-standard practice (12-factor app methodology) that separates configuration from code
- The pattern was detected with 90.70% confidence across 6 files, demonstrating consistent adoption and proven effectiveness in the codebase
- This approach enables secure credential rotation without code changes and supports different credentials per environment
- Prevents accidental exposure of secrets in version control, code reviews, and public repositories

## Consequences

Positive:
- Enhanced security posture by eliminating hardcoded credentials from source code
- Simplified credential rotation and management across multiple environments
- Reduced risk of accidental credential exposure in version control systems
- Improved compliance with security best practices and regulatory requirements
- Better separation of concerns between application logic and configuration

Negative:
- Requires additional setup and documentation for developers to configure environment variables locally
- Increases complexity of deployment processes requiring proper environment variable injection
- Potential for runtime failures if environment variables are not properly configured
- May require additional tooling or infrastructure for secure environment variable management in production

## Alternatives

- Store API credentials in configuration files (e.g., config.json, settings.yaml) excluded from version control (rejected)
  Rejected because: Configuration files are more prone to accidental commits, harder to manage across environments, and don't integrate well with container orchestration and CI/CD systems
  When valid: May be acceptable for local development with proper .gitignore rules, but not recommended for production
- Use dedicated secret management services (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault) with direct SDK integration (deferred)
  Rejected because: Adds infrastructure dependencies and complexity; can be layered on top of environment variable approach
  When valid: Recommended for large-scale production deployments with strict compliance requirements; these services can inject secrets as environment variables
- Hardcode credentials with encryption in source code (rejected)
  Rejected because: Encryption keys must still be managed securely, creating a circular dependency; violates security best practices and makes credential rotation difficult
  When valid: Never valid for production systems

## Risks

- Environment variables may be logged or exposed through error messages, stack traces, or debugging tools
  Mitigation: Implement logging filters to redact sensitive environment variables; use structured logging with secret masking; avoid printing environment variables in error messages
  Owner: Engineering team
- Developers may not properly configure environment variables in local development, causing integration failures
  Mitigation: Provide comprehensive documentation, .env.example templates, and clear error messages when required environment variables are missing; implement startup validation checks
  Owner: Engineering team and DevOps
- Environment variables in CI/CD systems may be accessible to unauthorized users or exposed in build logs
  Mitigation: Use CI/CD platform secret management features (GitHub Secrets, GitLab CI/CD variables); restrict access to secret configuration; ensure secrets are masked in logs
  Owner: DevOps and Security team

## Implementation Notes

- Use os.environ.get() or os.getenv() in Python with appropriate default handling and validation
- Create a .env.example file documenting all required environment variables with placeholder values (e.g., GITHUB_API_TOKEN=your_token_here)
- Add .env to .gitignore to prevent accidental commits of actual credentials
- Implement startup validation that checks for required environment variables and fails fast with clear error messages
- Document environment variable requirements in README.md and deployment documentation
- Use consistent naming conventions: SERVICE_NAME_API_KEY, SERVICE_NAME_TOKEN, SERVICE_NAME_SECRET

## Continuation Context


Verify commands:
- grep -r "api[_-]key\s*=\s*['\"]" --include="*.py" --exclude-dir=tests --exclude-dir=.venv | grep -v "os.environ" | grep -v "getenv"
- grep -r "token\s*=\s*['\"]" --include="*.py" --exclude-dir=tests --exclude-dir=.venv | grep -v "os.environ" | grep -v "getenv"
- find . -name ".env" -not -path "*/node_modules/*" -not -path "*/.venv/*" | xargs git check-ignore -v

Accept when:
- No hardcoded API credentials are found in source code (excluding test mocks)
- All .env files containing actual secrets are properly excluded from version control
- All external API integrations retrieve credentials from environment variables at runtime
- Documentation clearly specifies required environment variables for each integration

## Enforcement

- Verified by: Automated code review checks scanning for hardcoded credentials in pull requests
- Verified by: CI/CD pipeline security scanning tools (e.g., git-secrets, truffleHog, detect-secrets)
- Verified by: Manual code review focusing on new external API integrations
- Verified by: Pre-commit hooks checking for potential credential exposure
- Violation handling: Pull requests containing hardcoded credentials must be rejected and credentials rotated immediately
- Violation handling: Security team must be notified of any credential exposure incidents
- Violation handling: Exposed credentials must be revoked and regenerated before code can be merged
- Violation handling: Incident post-mortem required for any production credential exposure
- Exception process: Exception requests must be submitted to security team with detailed justification
- Exception process: Tech lead and security team approval required for any exceptions
- Exception process: Approved exceptions must be documented in code comments with expiration dates
- Exception process: Exceptions are reviewed quarterly and must be re-approved or remediated