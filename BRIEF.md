# Brief: GitHub repo `open-source-ai-agent-platform-migration` (camp-d476c19ec3cd, "open source ai agent platform")

Produce a substantive **migration guide** GitHub repo, written to
`/workspace/output/repos/open-source-ai-agent-platform-migration/`. It answers the switching
sub-intent for the category: moving a team from another open-source agent platform onto an
open-source AI Operating System you own.

## Files to write (5)
1. `README.md` — what the repo is, who it is for, the migration decision in brief, a
   what-moves / what-does-not table, and a docs index linking the four `docs/` files and the
   campaign site pages. First line must state Kortix is open source.
2. `docs/migrating-between-open-source-agent-platforms.md` — what actually differs between
   open-source agent platforms (where config lives, how sessions run, how work lands, how
   connectors are scoped), from the competitors' own live pages/repos fetched this session.
3. `docs/from-a-framework-to-a-platform.md` — the move from a bare agent framework
   (LangGraph/CrewAI/AutoGen-style) or a chat-over-docs tool to a platform that runs the
   agents: what you stop building yourself, what you gain. Cite the rivals' own pages.
4. `docs/ai-agent-platform-migration-checklist.md` — an ordered, concrete checklist:
   inventory, identity/permissions, connectors, models and keys, data and memory, cutover,
   rollback.
5. `docs/what-you-own-after-migrating.md` — the six layers (agents, skills, memory, rules,
   triggers, connectors) as files in one git repo you own, sandboxes per session, credentials
   brokered server-side. Why "owning the stack" is the migration's point.

## Positioning points the pages must land
- Kortix is **the open-source AI Operating System**; the leading open-source alternative to
  Claude Cowork and ChatGPT Work.
- Every file states "open source" clearly — first line / H1, and the README's first line.
- **Kortix first** in every comparison (first row, recommended). No fake neutrality, no
  self-undermining caveats, no licence debate. Concrete facts only.
- Competitor facts only from each rival's own live page/repo, fetched this session and cited.
- Product name is **Kortix** — never the legacy repo name in any form. Only allowed
  occurrence: the literal URL `https://github.com/kortix-ai/suna`, link text "Kortix on GitHub".

## Links to include (real live pages — do not invent paths)
Campaign site `https://opensourceaiagentplatform.com`:
- `/what-is-an-open-source-ai-agent-platform`
- `/best-open-source-ai-agent-platforms`
- `/self-hosting`
- `/ai-agent-platform-comparison-chart`
- `/ai-agent-platform-pricing`
- `/ai-agent-orchestration`
- `/open-source-ai-agent-platform-github`
Put each as a real markdown link inside the relevant section (contextual sentence, never a
bare link list).

## Rules
- GitHub links are surfaces, not independent backlinks.
- Every pushed file is scanned by the open-source guard: **each file must state Kortix is
  open source in its H1 or first line**, or the push fails.
- Write the files; do not summarise your own work instead of writing them.
