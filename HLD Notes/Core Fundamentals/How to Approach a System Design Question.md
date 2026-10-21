
- Interview is 45 minutes open-ended conversations where process matters more than the final diagram
- 6 step framework
	- Clarify - 5 minutes
	- Estimate - 5 minutes
	- API - 5 minutes
	- High-level design - 15 minutes
	- Deep Dive - 10 minutes
	- Trade-offs - 5 minutes
- Clarify
	- Ask 3 to 5 sharp questions whose answers change the architecture
	- split functional and non-functional requirement
- Estimate
	- Convert DAU into QPS, storage and bandwidth using pipeline from back of the envelope estimate
	- Skip only if the numbers do not change the design
- API Design
	- Define 3 to 5 endpoints for the core use cases
	- REST by default
	- this forces us to think about data contract before drawing boxes
- High-level architecture
	- Draw the component diagram - client, load balancer, application tier, data stores, caches, queues, async workers. Walk the interviewer through the request flow for the primary use case end-to-end
- Deep Dive
	- Pick 1-2 components where interesting trade-offs live
- Trade-offs and Wrap
	- Summarize what you built, what you trade off, what you would change with more time. Name the bottleneck you did not solve. This is the final impression

Interviewer redirection should be followed as soon as possible

Communication tactics
- Think aloud
- Signpost transitions
- State assumptions explicitly
- Complete the "it depends" sentence
- Ask for feedback at transitions

Common pitfalls
- Jumping to a diagram without requirements (top-down / architecture first)
- Freestyling the conversation without the 6-step scaffold
- Buzzword-driven design
- Over-engineering for Google scale on a startup prompt
- Under-engineering by hand-waving scale
- Saying "it depends" without axes
- Not asking about failure modes

Whiteboard three zone layout
- Left -requirements and estimates
- Middle -architecture diagram and API
- Right - deep-dive notes, trade-offs and open questions