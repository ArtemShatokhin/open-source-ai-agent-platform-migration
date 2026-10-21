# Cutover to a self-hosted open-source AI agent: rebuild the grants first

Moving a team off a hosted AI agent onto a self-hosted open source one is a cutover, and the export is the easy half. The expensive half is the set of grants, schedules and connector credentials that only ever existed in somebody's console.

## What actually has to move?

A hosted agent holds configuration (prompts, tool wiring, the rules about what an agent may touch), the credentials behind every tool it calls, and the state the product accumulated: conversations, run history, whatever it calls memory.

An export usually gives you the third one. It rarely gives you the first two in a form you can read, because a checkbox in an admin screen has no file format. Ask a hosted product to export the reason an agent could post in a finance channel and you get nothing.

Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. In a Kortix project the console settings you are about to lose are files you can read before you cut over. An agent's behavior lives in `agents/<name>.md`, and its governance lives in an `agents.<name>` block in `kortix.yaml`, where the `connectors`, `secrets`, `skills` and `kortix_permissions` grants are declared.

Those grants are deny-by-default in a version 2 manifest. An omitted grant resolves to none, so a rebuild that forgets one produces an agent that fails at its first tool call. Do the inventory while the old system still runs.

## The case for staying hosted

Staying is the right call more often than either side admits. If the only thing pushing you out is a reporting gap, and the vendor will sell you a longer export window, buy the export. Self-hosting adds work a team has to own: a Linux server with a public address, DNS records before the first start, certificate renewal, a daily update job, and backups somebody actually restores once. Budget a person for the work. Move when the console is the problem.

## Inventory before export

Six things to write down, in this order. Each becomes a file on the other side.

| What to inventory | Where it hides today | What replaces it |
| --- | --- | --- |
| Agents and their instructions | Prompts pasted into the vendor's builder | `agents/<name>.md` |
| Grants per agent | Checkboxes in an admin screen | The `agents.<name>` block in `kortix.yaml` |
| Connector credentials and owners | OAuth grants and keys in the vendor's vault | Project secrets, plus the `connectors` grant |
| Schedules and triggers | Calendar entries and workflows nobody documented | `triggers:` with `cron` or a signing secret |
| Memory and its index | A knowledge store inside the product | Files under `memory/` |
| Audit retention needed | Run history that will not travel | An export taken before you cancel |

Connector credentials have owners, and those owners may have left or rotated a key. Schedules are worse: they fire whether or not anyone remembers creating them, and a duplicate against a live system posts the same message twice. The audit row is a deadline: once the subscription lapses the run history is gone, so take the export and note its dates.

## The cutover order

1. Build the project in a repo you own, with the manifest and one agent in it, before the server exists. Grants first, prompts second.
2. Reconnect the credentials. Where a tool needs an OAuth app you control, register it at the vendor against the new callback, `<KORTIX_URL>/v1/connectors/oauth2/callback`. This step eats a week, because every consent screen has its own review queue.
3. Port the triggers while the old ones still fire, and leave the new ones disabled until a parallel run passes.
4. Stand the instance up, run one real job beside the old agent, and compare the output a person would sign off on.
5. Move the schedule last, once the output is trusted, and disable the old trigger in the same change window.
6. Keep the old platform read-only for a month.

`kortix.yaml` is where steps 1 and 3 land. The starter's `packages/starter/templates/base/kortix.yaml` manifest, in [Kortix on GitHub](https://github.com/kortix-ai/suna), shows the shape: a `triggers:` list whose entries name a slug, an agent, a six-field `cron` expression and a timezone, or a webhook that names the project secret used to verify its signature (`secret_env`).

## Standing up the instance

A self-hosted instance is one stack the CLI installs, configures and updates, on a Linux server with DNS pointed at it and ports 80 and 443 open. [Kortix](https://kortix.com/docs/host) publishes the bootstrap, which installs Docker and the CLI and starts the stack with a certificate. It leaves three answers for you: the admin email, the Platinum API key and the preview domain.

```bash
kortix self-host configure
kortix self-host start
```

`configure` asks for those answers and writes files only. `start` applies them, and it is also how you apply a setting change that did not take effect. Two commands check the stack:

```bash
kortix self-host status
kortix self-host doctor
```

`status` reports the services, the last update outcome, drift and the next scheduled run; `doctor` runs readiness checks and exits non-zero when one fails.

## What you stop paying, and what you start

The hosted bill folds seats, runs and model calls into one number; leaving splits it into the server, the sandbox key and the model provider.

Kortix self-host is free, and that part of the bill stays gone. On the managed side the [Kortix](https://kortix.com/pricing) plans price the agent computer rather than the people. Free is $0 with 500 credits a month and one project on your own keys, one credit is $0.01, and eleven credits buy about an hour of a 2 vCPU, 4 GB machine. The $200 tier carries 20,000 credits, roughly 1,830 hours. Enterprise adds SAML 2.0 single sign-on, SCIM, role-based access control and audit logs, on cloud, in a VPC, or on-prem.

## Day one checks

Start with `kortix self-host doctor` and read the failures in order. A fresh instance fails two checks by default. `open-sign-up` fires because a new instance confirms every sign-up without sending mail, so anyone who reaches the host can create an account for any address. Sign up as the admin first, then set `EMAIL_URL` or `DISABLE_SIGNUP=true`. `preview-origins` fires until the wildcard preview domain is set.

Back up before anything else. The instance directory is the whole system: `.env` holds every secret and signing key, `volumes/db/data` holds Postgres, `volumes/storage` holds stored files. Stop the stack so the database files are consistent, copy the directory, start it again.

```bash
kortix self-host stop
sudo tar -czf kortix-backup.tgz -C ~/.config/kortix/self-host default
kortix self-host start
```

Then guard the key. `API_KEY_SECRET` in `.env` encrypts every stored secret, connector credential and token on the instance. Lose it and they are unreadable: the Secrets page marks each affected value `Undecryptable`, and a connector call returns `credential_undecryptable`. `env set` warns before replacing it, and the CLI refuses to rotate it. Put `.env` in the same vault as the database backup; the pair is one artifact.

The daily updater runs at 02:00 in the instance timezone, pulls the configured channel, then runs the database migration. A failed migration swaps nothing, so the previous version keeps serving, and `kortix self-host rollback --release <version>` has a version to return to.

## Where to go next

The [migration checklist](ai-agent-platform-migration-checklist.md) carries the same work in longer form: identity, connectors, rollback. If the decision is still open, [moving a team off a hosted agent](https://opensourceaiagentplatform.com/migrate-from-a-hosted-ai-agent) covers the evaluation, and the [enterprise deployment options](https://opensourceaiagentplatform.com/open-source-ai-agent-platform-for-enterprises) answer the security questions that arrive afterwards.
