
### Chap 1. Introduction to Cloud Native

- A distributed system is a system in which individual computers are connected through a network and appear as a single computer.
	- Network is not so reliable every time and network failure needs to be consider.
	- Latency needs to be considered in case of distributed systems world.
	- There is limited network bandwidth and its not infinite.
	- network is not secure at all.
	- The topology is not static at all in case of distributed computing.
	- There is no any single administrator in case of cloud native distributed systems.

- CAT Theorem
	- Consistency, Availability vs. Isolation

- The Twelve-Factor App
	- The Twelve-Factor app methodology can be considered the foundation for cloud native applications and was first introduced by engineers at Heroku.
	1. Codebase
		- One codebase tracked in revision control; many deploys.
	2. Dependencies
		- Explicitly declare and isolate dependencies.
	3. Configuration
		- Store configuration in the environment.
	4. Backing Services
		- Treat backing services as attached resources.
	5. Build, Release, Run
		- Strictly separate build and run stages.
	6. Processes
		- Execute the app in one or more stateless processes.
	7. Data Isolation
		- Each service manages its own data.
	8. Concurrency
		- Scale out via the process model.
	9. Disposability
		- Maximize robustness with fast startup and graceful shutdown.
	10. Dev/Prod Parity
		- Keep development, staging and production as similar as possible.
	11. Logs
		- Treat logs as event streams.
	12. Admin processes
		- Run admin and management tasks as one-off processes.

- SLAs are important and defined in terms of 9s availability.

### Chap 2. Fundamentals
