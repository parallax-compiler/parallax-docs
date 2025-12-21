# ADR-002: Vulkan as GPU Backend

**Date**: 2025-12-22  
**Status**: Accepted  
**Deciders**: @fedres

**Context**: We must choose a GPU API that provides true vendor neutrality while maintaining performance. Options: OpenCL, Vulkan Compute, WebGPU, or vendor APIs.

**Decision**: Use **Vulkan Compute** as the sole GPU backend.

**Rationale**:
- **Universal support**: AMD, NVIDIA, Intel, Qualcomm, ARM Mali all ship Vulkan drivers
- **Future-proof**: Actively developed, industry standard, not deprecated like OpenCL
- **Performance**: Explicit control over synchronization, memory, and queues
- **SPIR-V ecosystem**: Standard intermediate representation, mature toolchains
- **No vendor lock-in**: Unlike CUDA/HIP, no single vendor controls the spec

**Alternatives Considered**:
- **OpenCL**: Deprecating on Apple platforms, fragmented vendor support
- **WebGPU**: Not ready for native HPC, limited compute features
- **Multi-backend (CUDA/HIP/SYCL)**: Explodes complexity, defeats purpose

**Consequences**:
- Must implement Vulkan loader and dispatch layer
- Need to test across multiple driver implementations (radv, nvidia, ANV)
- SPIR-V generation becomes critical dependency
- Requires Vulkan 1.3+ baseline (acceptable for 2025+ hardware)
