# ADR-005: C ABI for Runtime

**Date**: 2025-12-22  
**Status**: Accepted  
**Deciders**: @fedres

**Context**: Runtime must be callable from compiler-generated code and potentially other languages. Options: C++ ABI, C ABI, or custom FFI.

**Decision**: Expose runtime API via **stable C ABI** (`extern "C"`).

**Rationale**:
- **Language bindings**: Enables Rust, Python, Julia wrappers later
- **Compiler independence**: Generated code doesn't depend on C++ standard library
- **Stability**: C ABI is stable across compiler versions
- **Debuggability**: Easier to trace from debugger
- **Symbol visibility**: Explicit control over exported symbols

**API Surface**:
```c
// Core API (version 1.0)
void* parallax_umalloc(size_t size, parallax_memory_flags flags);
void parallax_ufree(void* ptr);
void parallax_sync(void* ptr, parallax_sync_direction dir);
parallax_kernel_t* parallax_load_kernel(const uint32_t* spirv, size_t words);
void parallax_launch(parallax_kernel_t* kernel, ...);
```

**Consequences**:
- Must version ABI explicitly (`PARALLAX_API_VERSION`)
- No C++ exceptions across boundary (use error codes)
- Memory allocation/free must be paired (no garbage collection)
- Documentation must provide C and C++ examples
