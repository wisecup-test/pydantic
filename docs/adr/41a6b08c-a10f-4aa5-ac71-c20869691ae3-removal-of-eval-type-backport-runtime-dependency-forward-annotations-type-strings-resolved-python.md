# Removal of eval_type_backport runtime dependency: Forward Annotations Type Strings Resolved Python

Status: proposed
Date: 2026-09-23
Deciders: AI (signal conversion)

## Context

- Historically, supporting modern typing syntax such as PEP 604 union operators (e.g., pipe syntax) across forward references in older Python runtimes required external backport packages.
- The eval_type_backport package was maintained as a direct runtime dependency to parse and evaluate forward-referenced type annotations during model schema generation.
- Advances in internal typing resolution utilities and standard library typing support make it feasible to retire external evaluation backports, reducing the project's external footprint and supply chain surface.

## Problem Statement

eval_type_backport introduces an external third-party runtime dependency solely to evaluate forward references and modern typing syntax, adding maintenance and supply chain overhead.

## Decision

1. MUST: Forward annotations and type strings MUST be resolved using Python standard library typing and internal typing utilities without third-party evaluation backports.

## Policy Block

- MUST Forward annotations and type strings MUST be resolved using Python standard library typing and internal typing utilities without third-party evaluation backports.

In scope:
- Runtime dependency configuration in package manifests and lockfiles
- Internal typing utilities and schema generation modules resolving forward references

Out of scope:
- Development or test-only typing inspection tools that are not runtime dependencies

## Rationale

- Eliminates a third-party runtime dependency, reducing security and packaging risks.
- Consolidates forward reference handling within internal typing utilities, allowing tailored control over PEP 604 union parsing and typing edge cases.
- Simplifies runtime dependency resolution and environment setup.

## Consequences

Positive:
- Decreased runtime dependency footprint and faster dependency resolution.
- Direct control over type string parsing and forward reference evaluation logic.
- Elimination of upstream breaking changes or latency from an external backport library.

Negative:
- Internal typing resolution logic must absorb maintenance of forward reference quirks and syntax compatibility.
- Potential risk of regressions in stringified PEP 604 union resolution on legacy runtimes if internal logic misses edge cases.

## Alternatives

- Using the eval_type_backport package to parse and evaluate forward-referenced type annotations (rejected)
  Rejected because: Retains an unnecessary third-party runtime dependency when forward references can be resolved using standard library typing and internal utilities.
- Retaining eval_type_backport as an optional extra for older Python runtime environments (rejected)
  Rejected because: Introduces fragmented runtime behaviors and increases testing matrix complexity across environments.

## Risks

- Breaking type resolution for stringified PEP 604 unions across existing model hierarchies
  Mitigation: Validate type evaluation across dedicated test suites covering forward references, type adapters, named tuples, and function validation.
  Owner: Core Framework Maintainers

## Implementation Notes

- Refactored forward reference evaluation across pydantic/_internal/_typing_extra.py, pydantic/_internal/_generate_schema.py, and pydantic/_internal/_model_construction.py.
- Removed eval_type_backport from pyproject.toml and updated uv.lock.
- Maintained and validated test coverage across tests/test_forward_ref.py, tests/test_main.py, tests/test_type_adapter.py, tests/test_types_namedtuple.py, tests/test_typing.py, and tests/test_validate_call.py.

## Continuation Context


Verify commands:
- Discover and execute the project dependency audit to verify eval_type_backport is not present in runtime manifests
- Discover and execute the test suite covering forward references, type adapters, and model construction

Accept when:
- Project dependency definitions and lockfiles contain no reference to eval_type_backport in runtime sections
- All forward reference, typing, and schema construction tests pass successfully without external backport dependencies

## Enforcement

- Verified by: Automated CI dependency audits ensuring eval_type_backport is absent from runtime dependency manifests
- Verified by: Unit and regression test suites validating forward reference and type string resolution
- Violation handling: Build failure during dependency manifest checks if external typing backports are added
- Violation handling: Pull request rejection for any reintroduction of external evaluation dependencies
- Exception process: Formal architectural review requiring demonstration that standard library and internal typing utilities cannot support a critical typing feature

## References

- commit:8a1a222b60131e517ed8f6c0387d296d181490c3
- file:pydantic/_internal/_generate_schema.py
- file:pydantic/_internal/_model_construction.py
- file:pydantic/_internal/_typing_extra.py
- file:pyproject.toml
- file:tests/test_forward_ref.py
- file:tests/test_main.py
- file:tests/test_type_adapter.py
- file:tests/test_types_namedtuple.py
- file:tests/test_typing.py
- file:tests/test_validate_call.py
- file:uv.lock
- pr:#13133