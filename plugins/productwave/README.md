# productwave

The ProductWave plugin for Claude Code. Its **`/productwave:shape-plan`** command turns a
**ProductWave bet — or any planned work** — into a high-level plan (a shaping brief)
through an adaptive, evidence-grounded interview — *before* your technical team refines it
into tickets. It pulls the matching customer evidence (supporting/challenging themes,
quoted signal, competitor intel) from ProductWave and interviews you one question at a
time, digging into the tensions in that evidence. You make the decisions; the command
records them.

## What you get

- The **`/productwave:shape-plan`** command.
- A connection to the **ProductWave MCP server** (declared by the plugin — nothing to
  configure by hand).

## Prerequisites

- **A ProductWave account.** The command reads *your* bets, themes, and evidence; it can't
  do anything without an account. It does not create one for you.
- On first use, Claude Code will open your browser to **authorize ProductWave** (standard
  OAuth). The connection acts *as you* — it only ever returns data your ProductWave access
  already allows. No tokens to paste, no shared credentials.

## Install

```
/plugin marketplace add full-scale-ventures/productwave-plugins
/plugin install productwave@productwave-plugins
```

Then authorize ProductWave when prompted on first run.

## Usage

Run it with the name (or URL) of the bet you want to shape:

```
/productwave:shape-plan <bet name>
```

**Run it from inside your product's own repository.** One dimension of the interview
grounds technical questions in real code — running from your repo lets the command cite
actual files. (It works without a repo too; you'll just skip that grounding.)

The interview covers problem/evidence reconciliation, target users, success signals,
scope, solution direction, technical seams, and risks — and produces a shaping brief you
can hand to your technical team.

## Shaping without a bet

You don't need a bet. Point the command at a **tracker artifact** — a Linear project or
epic, a Jira epic, an issue — or just describe the work:

```
/productwave:shape-plan ENG-412
/productwave:shape-plan a self-serve way for admins to rotate their own API keys
```

If a tracker MCP is connected, the command pulls the artifact's title and body itself;
otherwise it asks you to paste them. Either way it matches that text against your
product's themes and competitor intel and interviews you against the evidence that comes
back — the same interview a bet gets.

Two differences worth knowing:

- **The evidence is a point-in-time snapshot.** It's matched live and stored nowhere in
  ProductWave, so a later run may match differently. The brief records the date it was
  matched.
- **Nothing is created in ProductWave** unless you ask. When the brief is done, the
  command *offers* to turn what you shaped into a Candidate bet — you approve the
  hypothesis and success metrics first, and it never creates one on its own.

## Where briefs are saved

By default, briefs are written under **`docs/shaping-briefs/`** in the current repo, one
subfolder per subject with the brief itself as `<slug>.md` inside it — so per-ticket files
and evidence exports can live alongside each brief. The subfolder is `<slug>-<token>/` for
a bet (`<token>` being the tail of its ProductWave UUID), `<slug>-<tracker-id>/` for work
that came from a tracker, or plain `<slug>/` for work you described. To use a different
location for your team, add a line to your repo's `CLAUDE.md`:

```
Shaping briefs live in: `docs/product-briefs/`
```

The command reads this line and both writes new briefs there and reads prior briefs from
there — so shaping gets smarter over time as your team accumulates briefs (it won't
re-ask what a previous brief already settled). On a fresh setup with no prior briefs,
that's fine; it simply fills in as you go. If the line is absent, the command offers to
add it for you on first run.
