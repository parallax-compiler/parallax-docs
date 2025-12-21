# ADR-006: Kernel Caching Strategy

**Date**: 2025-12-22  
**Status**: Accepted  
**Deciders**: @fedres

**Context**: SPIR-V compilation and pipeline creation is expensive (50-500ms). Need aggressive caching.

**Decision**: Implement **three-level caching**:
1. **In-memory hash map**: `siphash(SPIR-V) → VkPipeline`
2. **On-disk cache**: `$HOME/.cache/parallax/kernels/v1/`
3. **Vulkan pipeline cache**: Use `VkPipelineCache` objects for driver-level optimization

**Cache Key Computation**:
```cpp
struct CacheKey {
    uint64_t spirv_hash;  // siphash of SPIR-V bytes
    uint64_t spec_const_hash; // Hash of specialization constants
    uint32_t device_id;   // Vendor/device ID
    uint32_t driver_version; // Driver version (invalidates on update)
};
```

**Eviction Policy**:
- In-memory: LRU with 1000 pipeline limit
- Disk: 10GB cap, evict oldest first
- Vulkan: Persistent across runs via `vkGetPipelineCacheData`

**Consequences**:
- Need atomic disk cache access (multi-process safe)
- Cache invalidation on driver updates (detect via `vkProperties`)
- Security: Cache directory must have restricted permissions (SPIR-V may contain IP)
- CI must disable caching for reproducible builds
