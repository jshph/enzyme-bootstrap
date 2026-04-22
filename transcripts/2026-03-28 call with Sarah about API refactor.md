---
people:
  - "[[Sarah Chen]]"
date: "[[2026-03-28]]"
---

# Call with Sarah about API refactor

Recorded Mar 28, 2026
Participants: [[Sarah Chen]], me

## Notes

### Key decisions
- Going with API gateway pattern over service mesh. Fewer failure modes at our team size, and [[Marco Reyes]]'s portfolio data backs this up.
- Cutover target: end of April. Feature freeze during migration window.
- [[Priya Kapoor]]'s developer portal needs to ship before cutover so teams know where to find the new API docs.

### Action items
- Sarah sends architecture doc by Friday
- I connect Sarah with [[Priya Kapoor]] for portal work
- Block a full day next week for dependency graph walkthrough

### Discussion notes
Sarah's main argument against the mesh: operational overhead is real, and at our team size we can't afford to debug mesh networking issues. The gateway gives up flexibility but the failure modes are more predictable.

Side thread on [[Elena Voss]]'s ambient knowledge transfer paper — Sarah sees a product insight in it. The best developer tools surface context without requiring search. Same principle as good API docs anticipating what you need.

---

## Transcript

Sarah Chen: So I've been going back and forth on the architecture, but after talking to Marco I think gateway is the right call.

Me: What pushed you off the mesh approach?

Sarah Chen: Honestly, failure modes. The mesh gives us more flexibility but we just don't have the team to debug networking issues at that layer. Marco mentioned three of his portfolio companies tried mesh at our scale and two rolled it back within six months.

Me: That's a strong signal. What's the timeline look like?

Sarah Chen: End of April for the cutover. Aggressive, I know. But if we freeze feature work during the window it's doable.

Me: Have you seen Priya's developer portal mockups? The portal should probably ship before the cutover.

Sarah Chen: I haven't actually. Can you bridge that intro? If teams don't know where the new docs live on day one, we'll be drowning in support requests.

Me: Done. I'll connect you two this week. Oh — did you see Elena's paper?

Sarah Chen: The ambient knowledge transfer one? Yeah, I read the sections you flagged. There's something in there about how the best systems surface context without requiring you to search. That's basically what good API docs do — anticipate what you need before you ask.

Me: That's exactly what I thought. Okay, action items: you send the architecture doc by Friday, I connect you with Priya, and we block a full day next week for the dependency walkthrough.

Sarah Chen: Works for me. Let me know once you've talked to Priya — I want to align on the portal timeline before I commit the team to the April target.
