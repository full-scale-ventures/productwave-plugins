# shape-bet

A Claude Code plugin that turns a **ProductWave bet** into a high-level shaping brief
through an adaptive, evidence-grounded interview — *before* your technical team refines
it into tickets. It pulls the bet's own customer evidence (supporting/challenging themes,
quoted signal, competitor intel) from ProductWave and interviews you one question at a
time, digging into the tensions in that evidence. You make the decisions; the command
records them.

## What you get

- The **`/shape-bet`** command.
- A connection to the **ProductWave MCP server** (declared by the plugin — nothing to
  configure by hand).

## Prerequisites

- **A ProductWave account.** The command reads *your* bets and evidence; it can't do
  anything without an account. It does not create one for you.
- On first use, Claude Code will open your browser to **authorize ProductWave** (standard
  OAuth). The connection acts *as you* — it only ever returns data your ProductWave access
  already allows. No tokens to paste, no shared credentials.

## Install

```
/plugin marketplace add full-scale-ventures/productwave-plugins
/plugin install shape-bet@productwave-plugins
```

Then authorize ProductWave when prompted on first run.

## Usage

Run it with the name (or URL) of the bet you want to shape:

```
/shape-bet <bet name>
```

**Run it from inside your product's own repository.** One dimension of the interview
grounds technical questions in real code — running from your repo lets the command cite
actual files. (It works without a repo too; you'll just skip that grounding.)

The interview covers problem/evidence reconciliation, target users, success signals,
scope, solution direction, technical seams, and risks — and produces a shaping brief you
can hand to your technical team.

## Where briefs are saved

By default, briefs are written to **`docs/shaping-briefs/`** in the current repo, one flat
file per bet. To use a different location for your team, add a line to your repo's
`CLAUDE.md`:

```
Shaping briefs live in: `docs/product-briefs/`
```

The command reads this line and both writes new briefs there and reads prior briefs from
there — so shaping gets smarter over time as your team accumulates briefs (it won't
re-ask what a previous brief already settled). On a fresh setup with no prior briefs,
that's fine; it simply fills in as you go. If the line is absent, the command offers to
add it for you on first run.
