# Parallax Documentation

Official documentation for the Parallax GPU offload compiler.

## Quick Links

- 📖 [Getting Started](docs/getting-started.md)
- 🚀 [Quick Reference](docs/quick-reference.md)
- 📊 [Performance Guide](docs/performance.md)
- 🔧 [API Reference](docs/api-reference.md)
- 💡 [Examples](../parallax-samples/)

## What is Parallax?

Parallax is an **automatic GPU offload compiler** for C++ parallel algorithms. It transforms standard C++20 code into GPU-accelerated code using Vulkan, **without requiring any source code modifications**.

### Key Features

- ✅ **100% ISO C++20 compliant** - Pure standard C++
- ✅ **Zero source changes** - Just use `std::execution::par`
- ✅ **Automatic GPU offload** - Compiler handles everything
- ✅ **Portable** - Works on NVIDIA, AMD, Intel GPUs
- ✅ **High performance** - 700+ million elements/second

### Example

```cpp
#include <vector>
#include <algorithm>
#include <execution>
#include <parallax/allocator.hpp>

int main() {
    std::vector<float, parallax::allocator<float>> data(1000000);

    // Runs on GPU automatically!
    std::for_each(std::execution::par, data.begin(), data.end(),
                 [](float& x) { x = x * 2.0f; });
}
```

No CUDA. No OpenCL. Just standard C++.

## Documentation Structure

### Getting Started
- [Installation](docs/installation.md)
- [First Program](docs/getting-started.md)
- [Quick Reference](docs/quick-reference.md)

### Guides
- [Performance Guide](docs/performance.md)
- [Best Practices](docs/best-practices.md)
- [Troubleshooting](docs/troubleshooting.md)

### Reference
- [API Reference](docs/api-reference.md)
- [Supported Algorithms](docs/algorithms.md)
- [Memory Management](docs/memory.md)

### Advanced
- [Architecture](docs/architecture.md)
- [Compiler Internals](docs/compiler.md)
- [Optimization](docs/optimization.md)

## Performance

Latest benchmarks on NVIDIA GTX 980M:

| Dataset | Throughput |
|---------|-----------|
| 1M elements | **744 M elem/s** |
| 100K elements | 228 M elem/s |
| 10K elements | 36.7 M elem/s |

See [Performance Guide](docs/performance.md) for details.

## Supported Algorithms

| Algorithm | Status | Performance |
|-----------|--------|-------------|
| `std::for_each` | ✅ Production | 744 M/s |
| `std::transform` | ✅ Production | 732 M/s |
| `std::reduce` | ⏳ Planned | - |

## System Requirements

- **GPU**: Vulkan 1.2+ capable
- **Compiler**: Clang 15+
- **OS**: Linux, macOS, Windows
- **RAM**: 4GB+ recommended

## GitHub Pages

View the documentation online: [https://YOUR_USERNAME.github.io/parallax-base](https://YOUR_USERNAME.github.io/parallax-base)

## Building Documentation

```bash
cd parallax-docs
# View locally
python3 -m http.server 8000
# Visit http://localhost:8000
```

## Contributing

See [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines.

## License

MIT License - see [LICENSE](../LICENSE)
