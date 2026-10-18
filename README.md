# Migrating to Kortix, the open-source AI Operating System

The leading open-source alternative to Claude Cowork and ChatGPT Work.

If your team already runs an open-source agent platform and the agent prompts, tool connections and schedules still live inside a vendor's canvas or a Python program, this repository is the migration path. Kortix is open source, and it is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers.

## Who this is for

It fits platform and engineering teams that have outgrown the first version of an open-source agent stack. Maybe you run CrewAI or LangGraph as a library and the orchestration is glue code your team maintains. Maybe you build on Dify or n8n and the configuration sits on a visual canvas inside the product. Either way, the parts you own are the agent definitions, the connections and the schedules, and on Kortix those become files in a repository you can read, diff and move.

The migration rewrites very few prompts. It changes where the configuration lives, where sessions run, and how finished work reaches your default branch.

## The migration decision in brief

Moving to Kortix answers four questions at once. Where does the configuration live? In a git repo you own, not in a vendor's database or canvas. Where do sessions run? Each session runs on its own isolated cloud computer, on its own branch. How does work land? Session work reaches `main` through a change request a person approves. Merge is default-deny for agents. Who bears the model bill? Any model provider with your own keys, or the ChatGPT plan you already pay for.

The comparison chart on the campaign site lays the field out dimension by dimension if you want the wider view before you start: [open-source AI agent platform comparison chart](https://opensourceaiagentplatform.com/ai-agent-platform-comparison-chart). When the target is your own infrastructure, the [self-hosting guide](https://opensourceaiagentplatform.com/self-hosting) walks the install commands.

## What moves and what does not

| Item | Outcome |
| --- | --- |
| Agent prompts and personas | Move into `agents/<name>.md` |
| Skills and scripts | Move into `skills/` |
| Knowledge and notes | Move into `memory/` files |
| Tool connections | Reconnect as connectors in `kortix.yaml` |
| Schedules and webhooks | Rebuild as `cron` or `webhook` triggers |
| Model keys | Re-add as project secrets |
| Visual canvas or workflow graph | Does not move; rebuild as files |
| Vendor run history and framework glue | Does not move; stays behind |

Two of those rows are worth reading twice. The vendor's run history and logs do not come with you, so export anything an audit needs before you cancel. And a visual canvas does not port as a file; you rebuild the same logic as an agent and a skill. Framework glue such as a hand-written graph or a task loop does not move either, because on Kortix the platform runs the agents and the repo defines them.

## The six layers you end up with

Kortix keeps six layers as text in one repo. Agents define who does the work, as markdown under `agents/`. Skills define how a job is done, as markdown plus scripts under `skills/`. Memory is the company brain, plain files under `memory/` today. Rules say what each agent may touch. Triggers say when work starts on its own, and there are exactly two kinds, `cron` and `webhook`. Connectors wire the tools the agents use. The document on [what you own after migrating](docs/what-you-own-after-migrating.md) walks each layer and the file it lives in.

## Docs in this repo

- [Migrating between open-source AI agent platforms](docs/migrating-between-open-source-agent-platforms.md): what actually differs between platforms, from each rival's own repo.
- [From a framework to a platform](docs/from-a-framework-to-a-platform.md): what you stop building when a platform runs the agents.
- [The migration checklist](docs/ai-agent-platform-migration-checklist.md): inventory, identity, connectors, models, data, cutover, rollback.
- [What you own after migrating](docs/what-you-own-after-migrating.md): the six layers as files, sandboxes per session, credentials brokered server-side.
- [From a solo agent to a team platform](docs/from-solo-agent-to-team-platform.md): why a team scales on the review gate, not agent count.

## Try it

Install the CLI, scaffold a project, then ship it:

```bash
curl -fsSL https://kortix.com/install | bash
kortix init
kortix ship
```

`kortix init` creates `kortix.yaml` plus your agents, skills and runtime config. `kortix ship` pushes the repo and brings it live. The source is public at [Kortix on GitHub](https://github.com/kortix-ai/suna), where the README carries the quickstart, the session loop and one agent's definition. If you would rather start with one job than a migration plan, the [repo map on the campaign site](https://opensourceaiagentplatform.com/open-source-ai-agent-platform-github) shows where each command lands.

Newer guides on the site answer the questions this repository raises: [AI agent permissions and policy](https://opensourceaiagentplatform.com/ai-agent-permissions-and-policy) for what an agent may reach, [the enterprise deployment options](https://opensourceaiagentplatform.com/open-source-ai-agent-platform-for-enterprises) a security review asks about, [moving a team off a hosted agent](https://opensourceaiagentplatform.com/migrate-from-a-hosted-ai-agent), and [builder or platform](https://opensourceaiagentplatform.com/open-source-ai-agent-builder-vs-platform).
