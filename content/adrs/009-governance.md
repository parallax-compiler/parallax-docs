# ADR-009: Community Governance Model

**Date**: 2025-12-22  
**Status**: Accepted  
**Deciders**: @fedres

**Context**: Open source project needs clear governance. Options: BDFL, committee, foundation.

**Decision**: Adopt **BDFL + Maintainer Council** model:
- **BDFL**: @fedres (you) - Strategic direction, architecture veto
- **Maintainers**: 3-5 core contributors - Merge rights, release management
- **Reviewers**: Per-module experts - Code review, design input
- **Contributors**: Everyone else

**Decision Making Process**:
1. **RFC**: Major changes require 1-week discussion on GitHub Discussions
2. **Lazy consensus**: No objections = approved after 7 days
3. **Veto**: BDFL can veto any decision (use sparingly)
4. **Appeal**: Veto can be overridden by 2/3 maintainer vote

**Contributor License Agreement**: 
- **No CLA**: Use DCO (Developer Certificate of Origin) with `Signed-off-by` 
- **Copyright**: Contributors retain copyright, license under Apache 2.0

**Code of Conduct**: 
- Adopt **Contributor Covenant 2.1**
- Enforced by maintainers
- Report violations to `conduct@parallax-compiler.org`

**Consequences**:
- BDFL time commitment for reviewing RFCs
- Need to recruit maintainers early
- Clear governance attracts enterprise users
- Avoids foundation overhead initially
