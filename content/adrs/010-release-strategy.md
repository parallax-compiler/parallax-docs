# ADR-010: Versioning and Release Strategy

**Date**: 2025-12-22  
**Status**: Accepted  
**Deciders**: @fedres

**Context**: How to version compiler, runtime, and plugins? Options: lockstep, independent, semantic versioning.

**Decision**: Use **Semantic Versioning (SemVer)** with **lockstep minor version**:

- **Format**: `MAJOR.MINOR.PATCH`
- **MAJOR**: Breaking ABI/API changes
- **MINOR**: New features (compiler, runtime, plugins bump together)
- **PATCH**: Bug fixes (can release independently)

**Versioning Scheme**:
```
parallax-compiler v0.1.0  → requires → libparallax-runtime v0.1.x
parallax-clang-plugin v0.1.0 → requires → parallax-compiler v0.1.x
```

**Release Cadence**:
- **Nightly**: `v0.1.0-nightly+20251222` (for brave users)
- **Monthly**: `v0.1.0` (feature releases)
- **Bi-annual**: `v1.0.0`, `v2.0.0` (LTS, ABI stable)

**Support Policy**:
- **LTS releases**: 12 months security fixes
- **Latest release**: 3 months active support
- **Older versions**: Community support only

**Packaging**:
- **Ubuntu/Debian**: PPA (`apt install parallax-compiler`)
- **Fedora/RHEL**: COPR (`dnf install parallax-runtime`)
- **Arch**: AUR (`yay -S parallax-git`)
- **Conda**: `conda install -c conda-forge parallax`
- **Spack**: `spack install parallax`
- **Docker**: `ghcr.io/parallax-compiler/parallax-ci:latest`

**Consequences**:
- Must maintain release branches
- CI needs to build packages for all platforms
- Version compatibility matrix becomes complex
- Need release engineer role (mechanize process)
