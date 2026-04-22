---
people:
  - "[[Elena Voss]]"
  - "[[James Okafor]]"
---

# What makes ideas neighbors

**[[Elena Voss]]** [[2026-02-18]]

Your question about "knowledge neighborhoods" got me thinking. The academic framing is that proximity in a knowledge graph implies relatedness, but that's circular. What actually makes two ideas neighbors?

I think it's shared context, not shared content. Two notes about completely different topics can be neighbors if they were created in the same cognitive state. [[James Okafor]] made a similar observation in his newsletter last month — he called it "thinking weather."

----

**me** [[2026-02-19]]

That framing is exactly what I needed. We've been struggling to explain why Enzyme's results feel right even when the topics don't obviously match — it's because the entity co-occurrence patterns are capturing shared context, not just shared keywords.

How do you measure cognitive context computationally though? That feels like the gap between your theoretical framework and something we could actually build.

----

**[[Elena Voss]]** [[2026-02-20]]

It is the gap, honestly. Current embeddings capture semantic similarity but completely miss temporal and emotional context. Your entity co-occurrence approach gets partway there — if two entities keep showing up together over time, that's a contextual signal even when the content is semantically different.

The hard part is the "emotional context" piece. I don't think anyone's cracked that yet. But co-occurrence over time might be a good enough proxy to start.
