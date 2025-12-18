# AI Operational Protocols

## Design Handoff Protocol (System First)

When integrating new designs from prototypes, mocks, or user requests, all AI agents must follow this "System First" extraction logic:

1.  **Map to Tokens**: Do not use raw hex codes. Convert specific hex codes in mocks to the nearest existing System variable.
    *   Example: `#BFA265` → `var(--gold)`
    *   Example: `#333` → `var(--text-dark)`

2.  **Map to Typography**: Do not use arbitrary font sizes. Snap hardcoded font sizes to the System Type Scale tokens.
    *   Example: `18px` → `1rem` / `var(--font-size-body)`
    *   Example: `32px` → `1.75rem` / `var(--font-size-h2)`

3.  **Flag Deviations**: Unexpected values (e.g., a new primary color or non-standard font weight) must be flagged to the user as potential *System Exceptions* before implementation. Ask: "This introduces a new style. Is this an exception, or should we update the global system?"

*Our prototypes drive the layout/content, but the Codebase drives the consistency.*
