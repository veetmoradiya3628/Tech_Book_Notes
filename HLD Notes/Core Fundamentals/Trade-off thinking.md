- System design works the same way. You have consistency, availability, latency, throughput, cost, simplicity, and developer velocity.
- The engineer's job is to name which ones matter most for this system at this scale, pick the architecture that fits, and say out loud what was sacrificed.

What a trade-off actually is 
- A trade-off is a design decision where improving one measurable property of the system necessarily degrades another.
- Physical
	- The speed of light caps intercontinental round-trip time to roughly 80 to 150 ms (e.g., US-to-Europe ~80 ms, US-to-Asia ~150 ms), so strong consistency across regions buys latency.
- Algorithmic
	- Every index that speeds reads slows writes, because each write updates both the row and the index.
- Economic
	- Hiring PostgreSQL operators is cheaper than hiring Spanner-equivalent operators, so self-hosted strong consistency buys operational cost.

|Axis pair|What you buy|What you pay|
|---|---|---|
|Consistency vs availability|Correct reads|Downtime during partitions|
|Consistency vs latency|Linearizable reads|Coordination wait (PACELC)|
|Read perf vs write perf|Fast queries (indexes, denorm)|Write amplification|
|Space vs time|Faster reads (caches, precompute)|Storage cost|
|Simplicity vs flexibility|Easy ops, fast onboarding|Fewer features, harder evolution|
|Cost vs performance|Lower bill|Higher latency or lower throughput|
|Dev speed vs maintainability|Ship today|Pay tomorrow in tech debt|
|Coupling vs decoupling|Easy coordination|Shared failure domain|

- Trade-offs compound. A choice for "consistency" often also means "higher latency" and "more operational complexity" and "more cost." Name all the downstream consequences, not just the first.
- The right way to say "it depends"
	- Name the dependency
	- State the branches
	- Recommend for this context

- ADR format

	```
	1. Context - the constraint
	2. Options - 2-4 candidates
	3. Chosen - active voice
	4. Why - link to context
	5. Given up - bane the loss
	6. Reversible ? - one-way or two-way door
	```

- Cognitive biases that ruin decisions
	- Sunk cost
	- Availability bias
	- Confirmation bias
	- Resume-driven development

- Two-way doors get a slack vote
- One-way door get an ADR or RFD

- For engineers: choosing a programming language for a new service is usually a one-way door (rewrite cost is huge). Choosing a cache eviction policy is usually a two-way door (change the config, restart). Choosing a database engine is somewhere in between, and the skill is recognizing which side it falls on for your specific system.

- The honesty test - one con can override multiple pros so our decision should be based on weighted decision matrix to expose the real disagreement

- When trade-offs change over time
	- Explore
	- Expand 
	- Extract

- Common pitfalls
	- It depends full stop - finish the options and its recommendations
	- Resume driven technology choices 
	- Ignoring two-way doors
	- Hiding what you gave up
	- Single dimension scoring
	- Sunk cost defense of bad choices

Take aways
- Every "it depends" needs a named dependency, a threshold, and a recommendation. Finish the sentence.
- Use the six-part template (Context, Options, Chosen, Why, Given up, Reversible?) for every significant decision. It fits in an ADR, a Slack message, or a 30-second verbal answer.
- Classify decisions as one-way or two-way doors. Two-way doors get fast, lightweight process. One-way doors get deliberate, written process
- Name what you gave up. If your design doc has only pros, you are writing marketing, not engineering.
- Cognitive biases (sunk cost, resume-driven, survivorship, confirmation) are organizational facts, not moral failings. Counter them with written artifacts and explicit weights.
- Trade-offs change over time. Beck's 3X model (Explore, Expand, Extract) gives vocabulary for "the right call last year is the wrong call now" without blame
- Sometimes the best decision is the null decision: keep what you have. "Technology we operate at 3 AM" is usually worth more than "technology that is new to us."

