# From an open-source agent framework to a platform: Kortix is open source and runs the agents

A framework gives you the agent loop as code you maintain. A platform runs those agents for you. Kortix is the open-source AI Operating System, and it is the platform layer: the agents, skills, memory, connectors and triggers are files in one git repo you own, and each session runs on its own cloud computer. This document covers what a team stops building when it moves from a bare framework, or a chat window over documents, to Kortix.

## What a framework hands you, and what it leaves to you

LangGraph describes itself as a low-level orchestration framework for building, managing and deploying long-running, stateful agents, and it ships durable execution, human-in-the-loop checkpoints and memory as library primitives ([LangGraph repo](https://github.com/langchain-ai/langgraph)). CrewAI describes itself as an open-source Python framework for building production-ready multi-agent workflows, with role-based Crews and event-driven Flows ([CrewAI repo](https://github.com/crewAIInc/crewAI)). Both give your engineers an agent abstraction inside your own code.

What neither ships is the machinery around the agent. You still stand up the runtime that starts a run. You decide where that run executes and how two runs avoid each other's files. You build the review path that lets a finished change reach production. You wire and guard the tool connections. A framework is a library: the library is the part you can read, and the machinery is the part you operate.

## What you stop building on Kortix

Kortix takes four of those jobs off your team.

The runtime and the schedule. Kortix starts sessions from the web, from Slack or Microsoft Teams, from the CLI, or from nobody at all when a trigger fires. A trigger starts a session with no person present: a `cron` schedule fires it on the clock, and a signed `webhook` fires it on an event.

Isolation per run. One isolated sandbox per session, and each session has its own isolated machine and branch, with the repo already cloned in. The agent can install, run and break anything inside that sandbox, and only what it commits survives. That is what lets thousands of agents run in parallel on one config without crossover.

The review and merge gate. When an agent finishes, it commits on its branch and opens a change request toward `main`. Session work reaches `main` through a change request a person approves. Merge is default-deny for agents. An agent can edit its own configuration on its session branch and propose the change. A person approves it.

The connector and credential layer. Connectors reach 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP, and connector credentials are brokered server-side and never enter the machine. You set each tool call to Allow, Ask or Block, down to the arguments it was given.

## What you gain that a library cannot give itself

Running the agents on a platform rather than in your process changes three things. Every session is a real machine, so an agent does work on files and tools instead of describing it. Every change arrives as a diff, so review means reading a change request rather than trusting a run. And the whole configuration is one repo, so you can `grep` your company, diff any change, and roll any part of it back.

The unit of work moves with it. On a framework the unit is your program, and the value is the code you wrote. On Kortix the unit is a session, and the durable value is the repo of agents, skills and memory the team accumulates.

## Where a framework still fits

A library remains the right tool for a narrow, code-owned job. If your team wants a multi-agent Python program it maintains end to end, CrewAI fits. If it wants a low-level graph with durable execution inside its own service, LangGraph fits. Kortix answers a different question: who runs the agents, where they run, and how their work reaches your default branch. Teams often keep both, and move only the company-wide work onto the platform.

The line between a framework and a platform is the one buyers get wrong most often. [What is an open-source AI agent platform](https://opensourceaiagentplatform.com/what-is-an-open-source-ai-agent-platform) works through the four questions that separate them: source, hosting, models and configuration.

## Frameworks consolidate, platforms persist

The framework layer moves under you faster than most teams expect. AutoGen's own repository now opens with a maintenance-mode notice: "AutoGen is now in maintenance mode. It will not receive new features or enhancements and is community managed going forward," and it points new users to Microsoft Agent Framework ([AutoGen repo](https://github.com/microsoft/autogen)). A framework you build on can enter maintenance, change its recommended successor, or fold into a managed product.

A platform you own survives that churn, because the thing you keep is the repo of agents, skills and memory, not the library that ran them. On Kortix the harness is OpenCode, a configured part of the repo, and the model is your choice with your own keys. When a framework changes hands or a model gets better, the move is a configuration change, not a rewrite of the company.

## Start with the checklist

The ordered steps live in [the migration checklist](ai-agent-platform-migration-checklist.md): inventory, identity and permissions, connectors, models and keys, data and memory, cutover, and rollback.
