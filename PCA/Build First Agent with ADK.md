
- Python setup
- .env file
- virtual env and activation
- `google-adk` package
- Gemini API key for LLM
	1. Gemini API via Google AI Studio
	2. Gemini via Google Cloud Vertex AI
- adk command line tool
- adk web to see the web UI interface for agent
- Agent = model + tool + orchestration
- adk run - terminal based interaction
- adk api_server - deploy as an API server
- `Agent` class
- `GOOGLE_API_KEY` - env var for Google AI studio authentication
- Flow to setup ADK development 

```
Create virtual environment → Install ADK → Get API key → Run adk create → Configure .env
```

- model
	- The underlying large language model (LLM) that powers your agent’s reasoning   and decision-making.
- name
	- A unique string identifier for your agent.
	- Identifies your agent internally within ADK
	- Critical in multi-agent systems where agents refer to each other
	- Used for logging, debugging, and agent delegation
- description
	- A concise summary of what   your agent does.
	- Used by other agents to decide   if they should route tasks to this agent
	- Helps in multi-agent systems where agents delegate to each other
	- Not used by the agent itself for its own behavior
- instruction
	- The behavioral blueprint that guides   how your agent acts and responds.
	- Defines the agent’s personality   and communication style
	- Specifies the agent’s core task or goal Sets boundaries and constraints on behavior Guides when and how to use tools   (covered in module 3) Shapes the output format

- ADK command-line tools look for a Python variable named root_agent as the entry point to your agent system. This is a convention that allows ADK to discover and run your agent.
- Always assign your main agent to a variable named `root_agent` , so ADK tools can find it.
- Three deployment methods
	- Terminal execution with adk run
	- API server with adk api_server
	- Programmatic execution with Python
- Two ways to define Agents
	- Python code
	- YAML based Agent

- Build Intelligent Agents
	- 