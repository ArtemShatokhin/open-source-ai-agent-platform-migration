# From a solo agent to a team platform: the open-source review gate

An open-source stack makes the second agent cheap. Keeping what every agent changed reviewable is the part a solo setup never had to formalize, and it decides whether a team can run the fifth one. Kortix is the open-source AI Operating System, and the platform worth owning at team size is the one where every agent change lands as a diff a person merges.

## What breaks first when a team adds agents

At one person, an agent that edits files on its own is a power tool. At five people the same behavior is an untracked change stream: prompts edited in place, connector calls nobody saw, and context one teammate fixed that the next never received.

The [open-source AI agent platform for teams](https://opensourceaiagentplatform.com/open-source-ai-agent-platform-for-teams) page argues the storage half of the fix: a shared repo of agents, skills, and memory instead of a private assistant per person. The gate is the other half: it decides which of the changes that repo collects reach `main`, and it is what turns a pile of agents into something a team can run.

## The mechanics that make every change a diff

A Kortix project is a git repo, and `kortix.yaml` sits at its root. The agents, the skills, the memory, the triggers, and the connectors live in it as files, so the configuration a team runs on is text it can read, review, and `grep`. The repo can be Kortix-managed, on GitHub, or on any other git host.

A session gives one agent a sandbox of its own. One isolated sandbox per session. Each session has its own isolated machine and branch. The branch takes its name from the session ID, so two sessions never collide.

When the run finishes, the agent commits its work, pushes that branch, and opens a change request toward the project's default branch. Session work reaches `main` through a change request. Merge is default-deny for agents.

The merge grant is a line in the repo. A person with `project.gitops.merge` reviews the change request and merges it, and what each agent may reach is declared in its `kortix_permissions` in `kortix.yaml`. The starter agent ships with `kortix_permissions: all`, so putting a person on every merge is one deliberate edit to a file the team already reviews.

## Review it from a terminal

The web app shows the same queue. From a terminal or a CI script, the review is three commands:

```bash
kortix cr ls        # change requests waiting for review
kortix cr diff 1    # read the session's diff
kortix cr merge 1   # land it on main
```

`1` is the change request number from `kortix cr ls`. To ask for another pass before it closes, `kortix cr request-changes 1 --message "…"` sends the note to the session that opened it.

## The gate covers the agent's own config and its memory

The review reaches the agent itself. An agent can edit its own configuration on its session branch and propose the change. A person approves it. A prompt that will steer next week's runs is a proposed change like any other, reviewed in the same place as code.

Company memory rides the same branch. Kortix keeps memory as plain files today, in a `memory/` directory next to `agents/` and `skills/`, so an edit to what an agent learned lands as a file change on its session branch. The edit reaches `main` through the change request a person reviews, and until that merge lands, other sessions run on the old memory. The page on [agent memory and state](https://opensourceaiagentplatform.com/agent-memory-and-state) walks the layout and the review path in detail.

## The tools an agent calls stay governed

A change request reviews what an agent wrote. A connector policy reviews what it did along the way. Each action can run, need approval, or be blocked, and the interface labels the three states Allow, Ask, and Block. Kortix checks the policy on every call, and the audit log records it.

Connector credentials are brokered server-side and never enter the machine. One credential serves the project, the manifest itself carries none, and every teammate runs on the same connection.

## What a team owns when the gate is files

The ownership question sits under all of it. Kortix's about page puts it plainly: every agent, every skill, every connector, the memory, and the whole configuration belong to the company that runs them, on its own infrastructure if it wants it there.

The same page frames the platform as six verbs: build, host, manage, and monitor today, with train and eval further out. The first four are the day-to-day job at team size, and each one has a home in the repo the gate protects.

## Put the first job behind the gate

A job earns its place when it comes back every week, needs context that outlives one session, and ends in output a person checks. A nightly support digest or a weekly report fits. The [open-source AI agent platform use cases](https://opensourceaiagentplatform.com/open-source-ai-agent-platform-use-cases) page shows the pattern card by card, with the artifact each job hands back, and the cards all share one ending: a person reads the result before it lands.

Start smaller than a migration. Take the job a teammate runs by hand each week, write the agent as a file under `agents/` with its reach in `kortix.yaml`, wire the two connectors it calls, and let its first output arrive as a change request. [What you own after migrating](what-you-own-after-migrating.md) maps the layers that follow, and [from a framework to a platform](from-a-framework-to-a-platform.md) covers what a team stops building when a platform runs the agents.

Install the CLI, clone a project, and let the first change reach you as a diff you merge: [Try Kortix](https://kortix.com).
