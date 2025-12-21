# Parallax Architecture: ISO C++ Compliance

## Critical Design Principle

**Parallax is 100% ISO C++ compliant. Users write pure standard C++ code.**

## User Code (Pure ISO C++)

```cpp
#include <algorithm>
#include <execution>
#include <vector>

int main() {
    std::vector<float> data(1000000, 1.0f);
    
    // Pure ISO C++ - no Parallax-specific syntax!
    std::for_each(std::execution::par, data.begin(), data.end(),
        [](float& x) { x *= 2.0f; });
    
    return 0;
}
```

## How Parallax Intercepts std::execution::par

### Option 1: Compiler Plugin (Recommended)
- Clang plugin intercepts `std::for_each(std::execution::par, ...)` at compile time
- Automatically generates GPU kernel
- Transparent to user

### Option 2: Library Preloading
- `LD_PRELOAD` intercepts standard library calls
- Redirects to Parallax implementation
- No recompilation needed

### Option 3: Header Replacement
- Provide custom `<execution>` header
- Template specialization for `std::execution::parallel_policy`
- Requires include path modification

## Current Implementation Status

**v0.3.0**: Uses internal `ExecutionPolicyImpl` with manual invocation
**v0.4.0 (Target)**: Full automatic interception of `std::execution::par`

## Compilation

### With Parallax (Automatic GPU):
```bash
clang++ -std=c++20 -O3 -fplugin=libparallax-plugin.so app.cpp -o app
./app  # Automatically uses GPU!
```

### Without Parallax (CPU only):
```bash
g++ -std=c++20 -O3 app.cpp -o app
./app  # Uses CPU
```

**Same source code, different compiler = GPU or CPU**

## No Custom Execution Policies

❌ **WRONG** (Not ISO C++):
```cpp
std::for_each(parallax::par, ...)  // Custom policy - breaks ISO compliance!
```

✅ **CORRECT** (ISO C++):
```cpp
std::for_each(std::execution::par, ...)  // Standard policy - ISO compliant!
```

## Implementation Notes

- `parallax::ExecutionPolicyImpl` is **internal only**
- Never exposed to user code
- Interception happens transparently
- Users write pure ISO C++
