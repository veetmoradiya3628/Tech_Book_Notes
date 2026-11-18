- What is Jenkins
	- Jenkins is an open source CI CD tool written in Java
	- It's an automation server used to build and deliver software projects
	- It has lot of plugins available
- CI/CD
	- verify and publish work by triggering automated build - test - for every commit or at least once a day
	- SDLC flow
		```
		Build - Test - Release - Deploy
		```

- Installation of Jenkins
	- Java is pre requisites for the jenkins

- Job
	- All automation task can be represented as Job
- Agent
	- VM or compute infra helper that can help jenkins to run jobs
	- Master Node & Agent Node
- Build Queue & Build Executor
- Item type
	- Freestyle project
	- Pipeline
	- Multi-configuration project
	- Folder
	- Multibranch pipeline
	- Organization Folder
- Freestyle project
	- General config
		- It allows for configuration such as discard old builds, use Github repo, take input from user during execution, limiting number of build execution based on time etc
	- Source code management
		- If want to connect to Github 
	- Triggers
		- Automated actions via scripts or hooks
	- Environment
		- Clean files before builds, Inject secret securely, timestamp in output etc
	- Build steps
		- Define tasks to execute during the build
	- Post-build actions
		- Define tasks to execute after the build
- Task
	- clone code, build and sent email notification on success or failure
- Email plugin - mailer
	- Email plugin setup requires App password and setup
- 