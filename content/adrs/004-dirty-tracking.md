# ADR-004: Dirty-Tracking Memory Model

**Date**: 2025-12-22  
**Status**: Accepted  
**Deciders**: @fedres

**Context**: We need unified memory without hardware page fault support. Options: full-copy, page-fault emulation, or dirty-tracking.

**Decision**: Implement **block-granular dirty-tracking** with 4KB blocks and bitset marking.

**Rationale**:
- **Hardware agnostic**: Works on all Vulkan devices, no HMM/XNACK required
- **Predictable performance**: No mysterious page-fault stalls
- **Fine-grained**: Only sync modified blocks vs entire buffer
- **Low overhead**: 64-bit bitset per 256KB (0.003% metadata)
- **Testability**: Deterministic behavior, easy to reason about

**Design Details**:
```cpp
// Metadata structure
struct UnifiedBuffer {
    void* host_ptr;
    VkDeviceMemory device_mem;
    std::atomic<uint64_t>* dirty_bitmap; // 1 bit per 4KB block
    size_t total_blocks;
};
```

**Consequences**:
- Must instrument memory writes (compiler inserts `rt_mark_dirty()` calls)
- False sharing possible (two threads write to same block)
- Copy-on-read needed for GPU→CPU sync (detect device writes)
- Need heuristic for block size tuning (runtime profiler will adjust)
