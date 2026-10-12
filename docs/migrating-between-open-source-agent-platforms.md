# Migrating between open-source AI agent platforms: Kortix is the open source one you keep

An open-source AI agent platform runs AI agents whose code you can read, on infrastructure you choose. Moving between two of them changes four things that decide what you own a year later: where the configuration lives, how a session runs, how finished work lands, and how a connector is scoped. Kortix is the open-source AI Operating System, and it answers all four the same way: files in a repo you own, one isolated sandbox per session, a change request a person approves, and rules per tool. The rivals below answer them differently, and each cell comes from that project's own repository.

## The four dimensions

Where the configuration lives decides whether you can move at all. On Kortix, agents, skills, memory, connectors and triggers are files in one git repo you own. On a visual platform the configuration sits on a canvas inside the product. On a framework it sits in your own code, which you own, and the maintenance comes with it.

How a session runs decides isolation. Kortix boots one isolated sandbox per session, each on its own branch, so thousands of agents can run in parallel without touching each other's files. A framework runs wherever you start it, and a coding-agent control center runs the agent on the machine you install it on.

How work lands decides safety. On Kortix, session work reaches `main` through a change request a person approves. Merge is default-deny for agents. A library gives you the graph or the crew and leaves deployment to you.

How a connector is scoped decides reach. Kortix sets rules per tool, down to the arguments of each call, and connector credentials are brokered server-side and never enter the machine.

## The platforms, side by side

| Dimension | Kortix | OpenHands | CrewAI |
| --- | --- | --- | --- |
| Open source | Yes | Yes | Yes |
| Where configuration lives | Files in one git repo you own | Conversations and automations in the control center | Python and YAML files in your repo |
| Where sessions run | One isolated sandbox per session, on its own branch | Your machine, Docker, VMs, or their cloud | Wherever you run your Python code |
| How work lands | Through a change request a person approves | The agent edits files on the machine you install it on | You run it and deploy it yourself |
| Connector scope | Rules per tool, down to the arguments of each call | MCP servers and tools you connect | Tools you wire in Python |

| Dimension | Kortix | Dify | LangGraph |
| --- | --- | --- | --- |
| Open source | Yes | Yes | Yes |
| Where configuration lives | Files in one git repo you own | On the visual canvas in the workspace | Python code in your project |
| Where sessions run | One isolated sandbox per session, on its own branch | Their cloud, your VPC, or self-hosted | Wherever you run your code |
| How work lands | Through a change request a person approves | You deploy the app you build | You deploy the graph you build |
| Connector scope | Rules per tool, down to the arguments of each call | Tools from the Dify Marketplace | Tools and nodes you code |

Kortix sits in the first column of both tables because it is the platform this repository recommends, and because it is the one built so the configuration leaves with you.

## What each platform actually is

Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. Each session gets its own isolated machine and branch, and the platform is what boots a configured agent into a session. [Kortix on GitHub](https://github.com/kortix-ai/suna) carries the README, the quickstart and the session loop.

OpenHands is a self-hosted developer control center for coding agents and automations, and its README describes it as a place to run OpenHands, Claude Code, Codex, Gemini, Pi, OpenCode, or any ACP-compatible agent across local, remote and cloud backends ([OpenHands repo](https://github.com/OpenHands/OpenHands)). It runs locally by default, with agent backends in Docker containers, on VMs, or within your company infrastructure, and its README warns that the agent-server gets full access to the filesystem of the machine you install it on. Its reach is coding work across repositories.

Dify is an open-source LLM app development platform, in its own repo's words, combining AI workflow, RAG pipeline, agent capabilities, model management and observability on one visual workspace ([Dify repo](https://github.com/langgenius/dify)). You build on the canvas and deploy on Dify Cloud, in your VPC, or self-hosted with Docker Compose; the configuration lives in the workspace.

CrewAI is an open-source Python framework with high-level abstractions and low-level APIs for building production-ready multi-agent workflows, in its own repo's words ([CrewAI repo](https://github.com/crewAIInc/crewAI)). Crews are role-based agents and Flows are event-driven automations, and a new project keeps agents in `agents/*.jsonc` with crew settings in `crew.jsonc`. The company sells a managed control plane, CrewAI AMP, on top of the framework.

LangGraph is a low-level orchestration framework for building, managing and deploying long-running, stateful agents, in its own repo's words, installed with `pip install -U langgraph` ([LangGraph repo](https://github.com/langchain-ai/langgraph)). It gives you durable execution, human-in-the-loop checkpoints and memory as library primitives, and the deployment path is LangSmith or your own infrastructure.

## Which differences decide the switch

The canvas against files question decides the switch more often than any feature list. A roundup that ranks open-source platforms by capability will not tell you where your configuration lives, and that is the one answer you cannot get back later. The shortlist on the campaign site sorts platforms by the team they fit, on the same ownership dimensions: [open source AI agent platforms compared](https://opensourceaiagentplatform.com/best-open-source-ai-agent-platforms).

A second difference is what a session is. On Kortix a session is a unit of agent work on its own cloud computer and branch, owned by whoever or whatever started it. On a framework, a run is your program. On a coding control center, a conversation is a coding-agent task. The noun a platform uses for a unit of work tells you what it thinks it is running.

## Notes on this comparison

Facts for each rival come from that project's own repository, checked October 2026. Star counts, release status and prices change, so check them at the source before a purchase. Kortix self-host is free, and Kortix Cloud is $40 per seat per month with 2,500 credits per month per seat, pooled, as written on the [Kortix pricing page](https://kortix.com/pricing).
