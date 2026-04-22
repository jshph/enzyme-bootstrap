---
people:
  - "[[Priya Kapoor]]"
  - "[[Sarah Chen]]"
---

# Intro to Sarah Chen

**[[Priya Kapoor]]** [[2026-01-20]]

Thanks for the intro to [[Sarah Chen]]. We had a great call about the migration UX. She thinks in systems, which is refreshing — most engineers I work with think in features.

One thing that came up: the transition period where both old and new interfaces coexist. That's where most migrations lose users. I'll put together some patterns for graceful degradation.

----

**me** [[2026-01-21]]

Glad that clicked. Sarah said the same thing about you — that you wanted to understand the architecture before touching the design. That's rare and we need it.

The coexistence problem is the one I'm most worried about. If you have patterns from Canopy that worked, we're all ears.

----

**[[Priya Kapoor]]** [[2026-01-22]]

We dealt with this at Canopy last year when we migrated our design system. The thing that saved us: we didn't sunset the old interface on a date. We sunset it when usage dropped below a threshold. Gave teams control over their own migration timeline.

I'll write up the pattern and send it to Sarah. Might apply directly to the API migration too.
