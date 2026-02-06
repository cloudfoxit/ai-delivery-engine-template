# Decisions: User profile page

## DEC-1: Relative date formatting

**Context:** The profile shows when the user joined. We need to display this in a human-friendly format.

**Options considered:**

1. **Custom formatter** — write a small utility that handles "X days/months/years ago".
2. **Library (e.g., date-fns, dayjs)** — use an established library's `formatDistanceToNow`.

**Decision:** Option 2 — use an existing library.

**Rationale:** Relative date formatting has many edge cases (pluralisation, boundary conditions). A well-tested library avoids subtle bugs and reduces maintenance.

## DEC-2: Avatar placeholder approach

**Context:** Users without an avatar need a visual placeholder.

**Options considered:**

1. **Static default image** — a generic silhouette.
2. **Initials-based placeholder** — generate a coloured circle with the user's initials.

**Decision:** Option 2 — initials-based placeholder.

**Rationale:** Initials provide visual distinction between users in lists and are more personal than a generic image. The implementation is lightweight (CSS + text).
