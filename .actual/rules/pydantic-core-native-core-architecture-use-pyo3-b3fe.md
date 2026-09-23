# pyo3 Library Adoption for Rust-Python Interoperability: Native Core Architecture Use Pyo3 Primary

These rules are ALWAYS ACTIVE for native data structures, validation modules interfacing directly with foreign runtime objects, and components implementing boundary type conversions, static string interning, or runtime exception translation.

### Rules

- **R-PYO3-001** MUST: The native core architecture MUST use pyo3 as the primary library for foreign runtime interoperability, object conversion, and boundary exception management across all native validation and input processing interfaces.
- **R-PYO3-002** MUST: Ensure that all runtime exception conversions translate directly into standard exception types at the boundary and that native panics are strictly caught or prevented.
- **R-PYO3-003** MUST: Utilize static thread-safe storage abstractions for any global interpreter references to avoid repeated runtime environment acquisition.
- **R-PYO3-004** MUST: Follow LOCK-VERSION GROUNDING before writing code that uses a versioned library by finding the dependency manifest, identifying the build tool, inspecting the repository lock or resolution artifact, looking up official docs for that exact version, confirming APIs exist, and re-running for version-sensitive behavior.

### Verify

```bash
# Discover and execute project build and test runner scripts from repository configuration
# Discover and execute linting and formatting verification tasks defined in workspace configuration
```

**Accept when:**
- All test suites verifying foreign-function interface type conversions, exception propagation, and thread-safe initialization pass without errors or panics.
- Repository static analysis and compilation checks succeed across all boundary modules without warnings regarding unhandled foreign runtime exceptions or unmanaged statics.

<enforcement>
Verified by continuous integration automated test suites exercising boundary conversion contracts and exception propagation, as well as mandatory peer code review. Pull requests introducing raw unmanaged foreign pointers, unhandled native panics at the boundary, or unapproved FFI libraries will be rejected. Compilation or test failures in boundary conversion modules block integration immediately.
</enforcement>