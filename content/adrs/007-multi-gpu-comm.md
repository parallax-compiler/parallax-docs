# ADR-007: Multi-GPU Communication Layer

**Date**: 2025-12-22  
**Status**: Accepted  
**Deciders**: @fedres

**Context**: Must support multi-GPU within node and across network. Options: NCCL, oneCCL, MPI, custom implementation.

**Decision**: Implement **layered communication stack** with auto-detection:
- **Layer 1**: Intra-device (memcpy)
- **Layer 2**: PCIe P2P (Vulkan `vkCmdCopyBuffer`)
- **Layer 3**: RDMA (libfabric + GPU direct)
- **Layer 4**: MPI (if available)
- **Layer 5**: TCP fallback

**Auto-Detection Algorithm**:
```cpp
comm_path detect_best_path(Device* src, Device* dst) {
    if (src == dst) return INTRA_DEVICE;
    
    // Check Vulkan peer memory
    VkPeerMemoryFeatureFlags features;
    vkGetDeviceGroupPeerMemoryFeatures(..., &features);
    if (features & VK_PEER_MEMORY_FEATURE_COPY_SRC_BIT) {
        return PCIE_P2P;
    }
    
    // Check if same NUMA node
    if (get_numa_node(src) == get_numa_node(dst)) {
        return RDMA; // Try GPUDirect
    }
    
    // Check MPI
    if (mpi_initialized && devices_in_same_comm(src, dst)) {
        return MPI;
    }
    
    return TCP;
}
```

**Consequences**:
- Adds dependency on libfabric (optional)
- Must handle MPI initialization/finalization conflicts
- Performance profiling needed to tune heuristics
- Security implications for TCP fallback (encryption? authentication?)
