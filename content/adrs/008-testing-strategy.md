# ADR-008: Testing Strategy and CI/CD

**Date**: 2025-12-22  
**Status**: Accepted  
**Deciders**: @fedres

**Context**: Project complexity demands comprehensive testing. Options: Unit tests only, integration tests, hardware CI.

**Decision**: Implement **four-tier testing pyramid**:

1. **Tier 1: Fast Unit Tests** (< 5 min)
   - Run on every PR
   - Mock Vulkan driver (`MockVulkanDevice`)
   - Test memory manager, kernel cache, AST matchers

2. **Tier 2: Integration Tests** (< 30 min)
   - Run on merge queue
   - Real Vulkan (SwiftShader software renderer)
   - End-to-end compilation and execution

3. **Tier 3: Hardware Tests** (< 2 hrs)
   - Nightly on self-hosted runners
   - AMD, NVIDIA, Intel GPUs
   - Performance regression detection

4. **Tier 4: Distributed Tests** (< 6 hrs)
   - Weekly multi-node
   - Cloud provider credits (AWS, GCP)
   - MPI + RDMA validation

**CI Infrastructure**:
- **GitHub Actions**: Free tier for unit/integration
- **Buildkite**: Self-hosted GPU runners
- **Cirrus CI**: Free macOS/ARM testing

**Test Coverage Requirements**:
- Unit tests: > 90% line coverage (LLVM parts exempt)
- Integration: All algorithms must have test
- Performance: < 5% regression threshold

**Consequences**:
- Need hardware donation program
- Must maintain multiple CI configs
- Flaky test detection (retry logic, statistical analysis)
- Requires significant initial infrastructure investment
