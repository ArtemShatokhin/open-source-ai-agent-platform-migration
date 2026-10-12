# The migration checklist: moving to Kortix, the open-source AI Operating System

Kortix is open source, and its configuration is files in one git repo you own. This checklist turns a move from another open-source agent platform into ordered work: inventory, identity and permissions, connectors, models and keys, data and memory, cutover, and rollback. Run the steps in order, because each one assumes the last.

## 1. Inventory what runs today

- List every agent in production with the job it does and the person who owns it.
- List every tool and API each agent reaches, and the credential it uses.
- List every schedule and webhook that starts work, with its trigger type and timezone.
- List every knowledge source an agent reads, and where that data lives today.
- Mark the agents that take a write action in a live system. Those migrate last.
- Export the vendor's run history and logs before you lose access to them.

## 2. Identity, permissions and governance

- Create the Kortix project and its git repo. `kortix init` scaffolds `kortix.yaml` plus your agents, skills and runtime config.
- Invite the people who will run it, and set each role.
- Map every agent's reach to a `kortix.yaml` block. The project brings per-resource permissions for people and agents. Roles, groups, and an audit trail come with it.
- Set each connector call to Allow, Ask or Block, down to the arguments of each call.
- Leave approval gates off until you decide where they belong. Approval gates you set. Off until you set them.
- Decide the merge grant. Merge is default-deny for agents. An admin can grant the merge capability.

## 3. Connectors

- Reconnect each tool as a connector: 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP.
- Confirm that connector credentials are brokered server-side and never enter the machine.
- Replace one broad token with per-tool rules, so an agent reaches only what its job needs.
- Recreate the write actions from step 1 with Ask set, so the agent stops with a person before it acts.

## 4. Models and keys

- Add each model provider with your own keys, or connect the ChatGPT plan you already pay for.
- Move model keys into project secrets. Secrets are encrypted at rest with a key per project.
- Set the model per agent, per session or per message, the way the old setup did.
- Check the new bill shape. On Kortix the provider invoices you directly, and self-host has no credit meter. The [pricing breakdown](https://opensourceaiagentplatform.com/ai-agent-platform-pricing) separates the software, hosting and model bills.

## 5. Data and memory

- Move knowledge into `memory/` as plain files, so it diffs with everything else.
- Decide what the old platform keeps. Its run history and logs do not travel.
- Put structured data behind a connector or a file an agent reads, not a store you cannot export.
- Keep secret values out of the repo. `kortix.yaml` declares secret names and grants, and secret values never appear in the repo.

## 6. Cutover

- Run the new agents beside the old ones on a single job first.
- Compare outputs, then fix prompts and skills before you widen scope.
- Move the schedule last, once the output is trusted.
- Watch a session live, and diff every change to an agent or a skill before it merges.

## 7. Rollback

- Keep the old platform running until the first month of sessions has been reviewed.
- Keep the old repo and its exports until an audit no longer needs them.
- Because the configuration is files in git, roll a change back with git instead of a support ticket.
- If you self-host, back up the Postgres data directory, the file storage directory, and the instance's `.env` file before any destructive command.
