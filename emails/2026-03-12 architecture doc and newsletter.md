---
people:
  - "[[Marco Reyes]]"
  - "[[Sarah Chen]]"
  - "[[James Okafor]]"
---

# Architecture doc and newsletter

**[[Marco Reyes]]** [[2026-03-12]]

[[Sarah Chen]] forwarded me the architecture doc. Clean design. My main question is about the embedding pipeline — how do you handle vault-scale changes without re-indexing everything?

Also, [[James Okafor]] pinged me about doing a writeup for his newsletter. Could be good exposure if the timing works with your launch.

----

**me** [[2026-03-13]]

On the embedding pipeline: we use incremental indexing. When a file changes, we re-embed only that file and update its edges in the entity graph. Full re-index only happens on schema changes, which are rare.

The newsletter timing is interesting. James and I have been talking about the pilot — if we publish the writeup right as the beta channel opens, it creates a natural funnel. Let me sync with him on dates.

----

**[[Marco Reyes]]** [[2026-03-14]]

Incremental indexing is the right call. My partners will want to understand the latency though — how long between a file change and the updated results showing up in a query?

On the newsletter: I'd actually push for publishing it closer to when we close the round, not when the beta opens. Better signal to other investors. But your call.
