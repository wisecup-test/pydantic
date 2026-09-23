# Consolidation of pydantic-core CI Workflows and Introduction of Zizmor Linting: Workflow Definitions Actions Not Maintained Within

Status: proposed
Date: 2026-09-23
Deciders: AI (signal conversion)

## Context

- Standalone GitHub Actions workflows and build actions previously resided inside the pydantic-core/.github/ subdirectory, separating core pipeline automation from the root repository workflows.
- Decentralized pipelines created duplicate workflow logic and separate maintenance surfaces across the Python and Rust boundaries. Consolidating build, test, and release pipelines into root-level workflows while integrating zizmor security linting centralizes CI configuration and provides static security analysis.

## Problem Statement

Decentralized CI workflows in subdirectories duplicate pipeline definitions, complicate workflow maintenance across the Python/Rust boundary, and lack centralized automated security linting.

## Decision

1. MUST_NOT: Workflow definitions and actions MUST NOT be maintained within subpackage directories such as pydantic-core/.github/.

## Policy Block

- MUST_NOT Workflow definitions and actions MUST NOT be maintained within subpackage directories such as pydantic-core/.github/.

In scope:
- Root repository GitHub Actions workflows (.github/workflows/)
- Root repository reusable actions (.github/actions/)
- Subpackage workflow locations (pydantic-core/.github/)

Out of scope:
- Third-party external actions referenced by workflows

## Rationale

- Centralizing workflows into root .github/workflows eliminates duplicated CI configurations across the repository.
- Moving custom actions like wheel building under root .github/actions allows shared access and unified maintenance.
- Enforcing zizmor static security analysis ensures automated identification of security vulnerabilities and antipatterns across all CI definitions.

## Consequences

Positive:
- Eliminates duplicate pipeline configurations across subdirectories.
- Provides unified visibility and centralized maintenance for both Python and Rust build and test steps.
- Enforces automated static security analysis across CI workflows.

Negative:
- May increase root CI run times and complexity of root workflow files.
- Creates tighter coupling between root repository workflows and core-only subpackage changes.

## Alternatives

- Decentralized GitHub Actions workflows maintained independently within the pydantic-core subdirectory (rejected)
  Rejected because: It causes duplicate pipeline configurations across the repository boundary and complicates consistent security auditing.

## Risks

- Consolidated root workflows may increase CI execution times or introduce tighter coupling during core-only development.
  Mitigation: Structure root workflow jobs and triggers to isolate core and Python-specific pipelines where appropriate.
  Owner: Infrastructure / CI Maintainers

## Implementation Notes

- Merged core-specific build and test actions from pydantic-core/.github/ into root .github/workflows/ci.yml.
- Relocated core wheel building action to .github/actions/core-build-pgo-wheel/action.yml.
- Added .github/zizmor.yml configuration to enforce CI security standards.

## Continuation Context


Verify commands:
- Discover and execute the project workflow security linter to validate all action and workflow configurations.
- Discover and execute repository structure checks to verify no workflow or action files exist in subpackage directories.

Accept when:
- Workflow security linter runs without errors or security warnings across all workflow files.
- No workflows or custom actions exist within pydantic-core/.github/.
- All build, test, and release jobs run successfully from root .github/workflows.

## Enforcement

- Verified by: Automated CI checks executing workflow linting
- Verified by: Pull request reviews verifying absence of subpackage workflow directories
- Violation handling: CI failure on pull requests introducing workflows outside the root directory or violating security lint rules
- Violation handling: Required remediation prior to merge approval
- Exception process: Documented exemption approved by repository maintainers and recorded in workflow configuration

## References

- commit:d8fbf4cc502338b5445e93be90f790c1b13e12ae
- pr:#13039
- file:.github/actions/core-build-pgo-wheel/action.yml
- file:.github/actions/people/action.yml
- file:.github/actions/people/pylock.toml
- file:.github/actions/people/requirements.in
- file:.github/dependabot.yml
- file:.github/workflows/ci.yml
- file:.github/workflows/codspeed.yml
- file:.github/workflows/coverage.yml
- file:.github/workflows/dependencies-check.yml
- file:.github/workflows/docs-update.yml
- file:.github/workflows/integration.yml
- file:.github/workflows/third-party.yml
- file:.github/workflows/update-pydantic-people.yml
- file:.github/workflows/upload-previews.yml
- file:.github/zizmor.yml
- file:.pre-commit-config.yaml
- file:pydantic-core/.github/actions/build-pgo-wheel/action.yml
- file:pydantic-core/.github/check_version.py
- file:pydantic-core/.github/workflows/ci.yml
- file:pydantic-core/.github/workflows/codspeed.yml
- file:pyproject.toml