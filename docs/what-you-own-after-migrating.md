# What you own after migrating to Kortix, the open-source AI Operating System

The migration's point is ownership. Kortix is open source, and after the move the company is a git repo you clone: the agents, the skills, the memory, the rules, the triggers and the connectors are files, not settings in someone else's database. This document maps what you hold after cutover and why each part matters.

## Six layers, one repo

Kortix keeps six layers as text in one repo. The table names each layer and the path it lives in.

| Layer | File it lives in | What it holds |
| --- | --- | --- |
| Agents | `agents/<name>.md` plus a `kortix.yaml` block | Behavior in markdown, governance in the manifest |
| Skills | `skills/` | Markdown plus scripts for one job |
| Memory | `memory/` | The company brain, plain files today |
| Rules | `kortix.yaml` | What each agent may touch |
| Triggers | `kortix.yaml` | When work starts on its own |
| Connectors | `kortix.yaml` | The tools an agent is wired to |

A Kortix project is a git repo, and the repo is the company: configuration and accumulated state in one place, all text, all under version control, readable by a person and editable by an agent. `kortix.yaml` is the Kortix layer, declaring the machine image, the connectors and triggers, and secret names and grants. The OpenCode config is the runtime the agents think in. Everything past that is files.

## Sandboxes per session

Every session runs on its own isolated cloud computer, on its own branch, with the repo cloned in. The agent can install, run and break anything inside that sandbox, and only what it commits survives. Because two sessions never share a machine or a branch, thousands of agents can run in parallel on one config. Coordination and isolation are the same mechanism here, and the [orchestration guide](https://opensourceaiagentplatform.com/ai-agent-orchestration) walks the checks that separate a setup you own from one that owns you.

## Credentials brokered server-side

Connector credentials are brokered server-side and never enter the machine. When a session calls a connected app, Kortix spends the credential outside the sandbox and hands the result back. Secrets are encrypted at rest with a key per project, and a secret has an exposure. `environment` is the default exposure, `egress-enforced` is the experimental network-boundary option, and `none` means no sandbox presence at all. Secret values never appear in the repo; `kortix.yaml` declares the names and the grants.

## Governance travels with the repo

Ownership includes the rules. Per-resource permissions for people and agents live with the project. Roles, groups, and an audit trail come with it. Approval gates you set. Off until you set them. Session work reaches `main` through a change request a person approves. Merge is default-deny for agents. Because the rules are files and project state is reviewable, a security reviewer can read how an agent is scoped instead of taking a policy page's word for it.

## Why owning the stack is the point

A hosted agent product hands you the same capability and keeps the durable part: the configuration, the memory and the run history. When you leave, you rebuild. Owning the stack inverts that. The repo is portable, so you can move the whole system to a new host by cloning it, diff any change, and roll any part back. The model is a commodity that changes monthly, and the repo is the part that compounds. The [repo map on the campaign site](https://opensourceaiagentplatform.com/open-source-ai-agent-platform-github) shows where the source and each part of the configuration live on GitHub.

## What stays with the vendor

Two things do not travel. The old platform's run history and logs stay behind, so export what an audit needs before you cancel. And a visual canvas does not port as a file; you rebuild the same logic as an agent and a skill. Everything the team accumulates after that, the prompts, the skills, the memory and the rules, lands in the repo you own.
