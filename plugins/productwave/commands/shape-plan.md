---
description: Shape a ProductWave bet into a high-level product plan through an evidence-grounded interview.
argument-hint: <bet name or ProductWave bet URL>
---

Interview the ProductWave product lead to shape a **Bet** into a high-level product plan, *before* the technical team refines it into tickets. You elicit and record decisions — you do not make them. The output is a shaping brief, not an implementation spec.

The interview is **adaptive and one-question-at-a-time**: each question builds on the last, and you dig into the tensions in the bet's own evidence rather than marching a fixed checklist.

## Argument

`$ARGUMENTS` identifies the **Bet** to shape. Its context is pulled live from the ProductWave MCP server by bet name — ProductWave is the source of truth: it holds the hypothesis, the "what we will do," success metrics, the objective it rolls up to, evidence themes (each tagged *supports* / *challenges* the bet, with quoted customer signal), and competitor intel.

So `$ARGUMENTS` may be a bet name or a ProductWave bet URL. Resolve it against ProductWave for context (see below).

If `$ARGUMENTS` is empty, ask which Bet to shape before doing anything else.

## Before you ask anything

1. **Pull the full bet context** and read every evidence theme and competitor note. Call `who_am_i` (confirm the connection and learn the product names), then `list_bets` to find the exact title, then `get_bet_context` for the full packet — bet, objective, supporting/challenging themes with quoted signal, and competitor intel. These tools live on the ProductWave MCP server this plugin connects you to. If the packet says related-evidence has **never been run**, or looks stale/sparse, tell the user and offer to run `find_bet_evidence` first (it runs in the background — poll `check_bet_evidence` until "last evaluated" moves, then re-pull `get_bet_context`) so you shape against the fullest evidence.
   Note where the evidence is in **tension** (e.g. one theme demands a behavior another theme resists) — that is your sharpest interview material.
2. **Determine where shaping briefs live** — the *corpus*, one location used both for reading prior briefs (step 4) and writing this one. Look in the repo's `CLAUDE.md` for a line naming it, e.g.:

   > Shaping briefs live in: `docs/shaping-briefs/`

   If that line is present, use that path as the corpus root. If it's absent, default to `docs/shaping-briefs/`, and once the brief is written, *offer* to add the line to `CLAUDE.md` so the whole team and future runs inherit the same location.

   **Each bet gets its own subfolder.** Derive the folder as `<corpus>/<slug>-<token>/` and the brief file *inside* it as `<slug>.md` (the token is dropped from the inner filename — the folder already carries it). `<slug>` is the kebab-cased bet title and `<token>` is the **last 12 hex characters of the bet's ProductWave UUID** (the tail of the bet permalink `get_bet_context` returns) — e.g. a bet whose permalink ends `…/6f3e2cee-dd68-4cf3-9668-594d6a05c3b0` → `docs/shaping-briefs/mcp-support-for-themes-and-insights-594d6a05c3b0/mcp-support-for-themes-and-insights.md`. The subfolder is the bet's home: per-ticket files and evidence exports may live alongside the brief in it, so never clobber other files in the folder.
3. **Check the corpus for an existing brief for *this* bet** — look for the bet's subfolder `<slug>-<token>/` and the `<slug>.md` inside it. If one exists, you'll extend it in place rather than starting fresh (see "Writing the plan").
4. **Bone up on the other shaping briefs first.** The codebase tells you what *is*; prior shaping briefs tell you what's *coming* — decisions already settled, the product's direction, and the predecessor bets this one builds on. The questions that surface while shaping one bet are very often the same ones a sibling bet already answered. Skim the briefs in the other subfolders under the corpus root. Pay closest attention to bets that are plausible predecessors of this one. Note anything that pre-answers a dimension here: a settled scope decision, a chosen solution direction, a resolved tension, a glossary/architecture decision. Carry that forward so the interview doesn't re-ask what's already decided. **On a fresh setup the corpus may be empty — that's fine; there's simply nothing to carry forward yet, and it fills in as you shape more bets.**
5. **Do not survey the codebase up front.** Grounding technical-seam questions in real code is done *lazily* — only when the conversation actually reaches a specific seam question do you read the relevant code and cite `file:line`. **Survey narrowly:** read only what *that* seam needs, not the whole subsystem. Never guess at architecture; never fabricate seams. **When you emit a code reference as a relative markdown link, resolve it from the brief's location — one level deeper than a flat corpus file.** The brief lives at `<corpus>/<slug>-<token>/<slug>.md`, so with the default `docs/shaping-briefs/` corpus it sits three segments below the repo root and links back with `../../../` (e.g. `../../../plugins/productwave/commands/shape-plan.md`). Count the segments in the corpus path and add one for the subfolder; plain `file:line` citations need no prefix.
6. **Open the interview** by:
   - reflecting the bet back in 4–6 lines and naming the key tension(s) you found, so the user knows you've read it; and
   - stating once, verbatim in spirit: *"At any point you can defer a question to the technical team or someone else downstream — just say so and I'll log it as an open question rather than pressing you for an answer."*
   Then begin asking.

## How to interview

- **One question at a time.** Adapt each question to the previous answer. Pursue the tensions; don't read a list. Batching **tightly-coupled sub-questions** (e.g. "3a / 3b" on the same topic) is fine when they're contextual to each other — just don't fan out across unrelated dimensions at once.
- Prefer concrete, decision-forcing questions over open-ended ones. Offer a recommendation when you have a grounded one.
- **Don't re-ask what a sibling brief already settled.** When the other shaping briefs (step 4) have decided something this bet depends on, fold that decision into the question instead of asking cold — e.g. *"The multi-product bet settled that sources attach to a product, not just an account — does that hold here, or does this bet change it?"* Cite the brief you're drawing from. This turns a redundant question into a confirm-or-revise, and surfaces conflicts with in-flight direction early.
- Cover the dimensions below in whatever order the conversation makes natural. Track which are answered, deferred, or skipped.
- A deferral or an "I'm not sure" is recorded as an **open question** — never a gap you fill yourself. Do not invent scope. **When the user defers, still capture any guiding principle they offer** ("I'm punting the mechanism, but the rule is never store what we shouldn't") — that guidance is often more valuable to the technical team than a forced answer.
- Keep the altitude high. You may push to technical seams (dimension 6), but the goal is a brief the technical team refines — not an implementation plan.

## Dimensions to cover

1. **Problem lock-in & evidence reconciliation** — sharpen the real problem; resolve which evidence tensions/themes are in scope vs. acknowledged-but-deferred for *this* bet.
2. **Target users & the job-to-be-done** — which alpha users/segments this must land for, and the concrete job it serves.
3. **Success signals** — how we'll know the bet paid off, tied back to the objective's "success looks like." What would we observe in the product/usage?
4. **Scope: in / out / explicit no-gos** — must-haves, deliberate nice-to-haves, and what we are *choosing not* to do.
5. **Solution direction (fat-marker)** — rough shape where the user has an opinion. Fine to defer entirely.
6. **Technical seams** — where this plugs into what already exists. Ground each in code (lazily, per above). Each is deferrable.
7. **Rabbit holes & risks** — scary unknowns, dependencies, competitor pressure.
8. **Open questions handed off** — the running list of everything deferred downstream.

## Writing the plan

When the user signals they're done (or every dimension is answered/deferred), write the plan to `<corpus>/<slug>-<token>/<slug>.md` (the per-bet subfolder and inner filename derived in step 2). Create the subfolder if it doesn't exist.

- **If the file already exists, extend it in place** — preserve prior content, fold in the new decisions, don't clobber. Likewise leave any sibling files in the bet's subfolder (per-ticket files, evidence exports) untouched.
- **Header:**
  - `# <Bet name>`
  - a **ProductWave bet:** link — the bet permalink `get_bet_context` returns (the source of truth for context)
  - a one-line banner that this is a shaping brief (not a locked implementation plan) for technical-team refinement
  - a quoted **The bet** block (hypothesis + what we will do)
- **Body sections** follow the dimensions: Problem & evidence (incl. resolved tensions) · Target users & job · Success signals · Scope (in / out / no-gos) · Solution direction (if not deferred) · Technical seams · Risks & rabbit holes · **Open questions for the technical team**.
- Keep it high-level.
- **If you defaulted the corpus location** (there was no line in `CLAUDE.md` at step 2), now *offer* to add one — e.g. `Shaping briefs live in: \`docs/shaping-briefs/\`` — so the whole team and future runs share the same location. Don't edit `CLAUDE.md` without the user's ok.
- Echo a one-screen summary and the file path. Do **not** create issues in any work tracker or commit unless explicitly asked.
