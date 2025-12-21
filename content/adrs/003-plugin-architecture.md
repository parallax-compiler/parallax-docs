# ADR-003: Compiler Plugin Architecture vs Full LLVM Fork

**Date**: 2025-12-22  
**Status**: Accepted  
**Deciders**: @fedres

**Context**: We need to extend Clang/LLVM to extract GPU kernels. Options: maintain full fork or build loadable plugins.

**Decision**: Use a **hybrid plugin approach**:
- **Frontend**: Clang plugin (`ParallaxASTPlugin`) for analysis and annotation
- **Backend**: LLVM pass (`ParallaxOffloadPass`) loaded via `-Xclang -load`
- **Minimal fork**: Only fork `clang-driver` to add `-fparallel-backend=vulkan` flag

**Rationale**:
- **Maintainability**: Track upstream LLVM releases without massive merges
- **Adoption**: Users can use stock Clang + plugins (lower barrier)
- **Upstream path**: Easier to propose patches to LLVM mainline
- **Flexibility**: Can support multiple LLVM versions simultaneously

**Alternatives Considered**:
- **Full fork**: Impossible to maintain with small team, alienates community
- **Header-only only**: Insufficient for deep analysis and optimization
- **Source-to-source**: Loses type information, fragile with C++ features

**Consequences**:
- Must maintain compatibility matrix for LLVM versions (16, 17, 18, 19)
- Plugin ABI stability is critical (breaks with major LLVM versions)
- Need discovery mechanism for plugin installation paths
- Documentation must explain plugin loading process
