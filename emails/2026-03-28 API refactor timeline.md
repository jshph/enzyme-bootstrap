---
created: 2026-03-28
people:
  - "[[Sarah Chen]]"
  - "[[Priya Kapoor]]"
  - "[[Marco Reyes]]"
---

# API refactor timeline

**[[Sarah Chen]]** [[2026-03-28]]

Hey — quick update on the API refactor. We ended up going with the gateway pattern instead of the mesh approach. Fewer moving parts, and [[Marco Reyes]] mentioned Foundry's portfolio companies have had better luck with gateways at our scale.

The team is targeting end of April for the cutover. I'll send you the architecture doc once [[Priya Kapoor]]'s team finishes the developer portal mockups.

----

**me** [[2026-03-28]]

Good call on the gateway. What does the feature freeze window look like?

Have you seen Priya's portal mockups yet? We should align before committing to April.

----

**[[Sarah Chen]]** [[2026-03-29]]

End of April, assuming we freeze feature work during the migration window. Haven't seen the mockups yet — can you bridge that intro?

If teams don't know where the new docs live on day one we'll be drowning in support requests.

----

**me** [[2026-03-29]]

Done — connecting you two this week. Let's block a full day next week for the dependency graph walkthrough.

----

**[[Priya Kapoor]]** [[2026-04-01]]

Sarah — jumping in here. First pass on the developer portal wireframes is done. Main insight from the design work: people don't want a dashboard, they want a feed. Something that shows what changed in the API and why it matters, not a static grid of endpoints.

Attaching the Figma link. Tuesday for a walkthrough?

----

**[[Sarah Chen]]** [[2026-04-01]]

This is great, Priya. Tuesday works. The feed concept maps to something [[Elena Voss]] calls "ambient awareness" — the system surfaces what's relevant without you having to go looking.

Let's build the portal around that.
