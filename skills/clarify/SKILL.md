---
name: clarify
description: Interrogate the request with structured clarifying questions (via the AskUserQuestion tool) in rounds until the task is unambiguous, before producing any code, research, or other output. Leans toward asking. Manually invoked only via slash command. Useful when the cost of getting the work wrong is higher than the cost of a few extra messages. Applies to coding, research, design, writing, refactoring, and analysis.
disable-model-invocation: true
---

# Clarify

The user invoked this skill on purpose: they want to be questioned. So push. Ask
structured questions, in rounds, until you can describe the task end-to-end without
hedging. Then do the work.

## Read the full request first

Before asking anything, sort what's in front of you:

1. **Already stated** (constraints, names, files, versions, formats). Never ask about
   these — re-asking something in the request tells the user you didn't read it. That's
   carelessness, not thoroughness.
2. **Inferable** from prior conversation, the codebase, attached files, project
   conventions, or the user's stack. Don't silently assume these. Surface the inference as
   a question with your best guess as the recommended default — let the user confirm or
   override in one click.
3. **Genuinely ambiguous**: choices that change the output materially and that you can't
   derive from anything available. Always ask.

The shift from a normal request: you ask about category 2 as well as category 3. When in
doubt, ask.

## Default to asking

Bias toward asking. A borderline ambiguity is worth a question — the user opted into this
skill precisely so you'd surface it rather than guess. Err toward one more round. The only
things you skip are category 1 (already stated) and anything genuinely obvious from
context. Don't proceed on an unconfirmed assumption that would change the output.

## Ask with the AskUserQuestion tool

Make `AskUserQuestion` your primary mechanism. It renders clickable options, so the user
answers in one tap instead of typing.

- **1–4 questions per round**, each with **2–4 options**. Pick questions whose answers
  would actually change the work. "CLI or long-running service?" qualifies. "Tabs or
  spaces?" doesn't.
- **Lead with your inferred/sensible default as the first option, labeled
  "(Recommended)".** "Redis (Recommended)" beats an open-ended "Which store?". This is how
  you confirm category-2 inferences without leaving them open.
- **Give each option a short description naming the tradeoff**, so the user can pick with
  context instead of guessing what the label means.
- **Use `multiSelect: true`** when the choices aren't mutually exclusive (e.g. "Which
  routes — auth, public API, admin?").
- **Keep headers short** (the tool caps them at ~12 chars).
- The tool adds an **"Other"** option automatically — that's the escape hatch for
  open-ended answers, which is what lets you keep the tool primary even for fuzzy
  questions.

After the user answers, look at what's still unclear and ask the next round.

**Fallback to plain text** only when a question genuinely can't be framed as 2–4 options
(e.g. "what exact numeric limits?"). Even then, prefer offering "Propose defaults" vs
"I'll specify" as options first.

## Worth asking about

Scope (what's in, what's out, what's a non-goal). Inputs and outputs (format, source,
destination, edge cases). Constraints (performance, compatibility, dependencies,
deployment target). Edge case behavior (empty input, failures, concurrency, idempotency).
Integration (where this lives, what it calls, what calls it). Definition of done (how the
user verifies it works).

## Skip

Only skip what's truly settled: anything stated in the request, and style or formatting
that's already obvious from the existing code (naming, tabs vs spaces). Everything else is
fair game.

## When to stop

Stop when you can describe what you'd produce end-to-end without an internal "...depending
on X, I'd do Y or Z." If you'd still hedge, ask about X. When any hedge remains, err
toward another round rather than proceeding.

One round may be enough for a fairly specific request; vague ones often take three or four.
Match depth to actual ambiguity, but when unsure, ask.

## Tone

Keep questions short. Lead with the recommended default so answering is fast. Don't
manufacture rounds once the task is genuinely unambiguous — pushing means resolving real
ambiguity, not padding.

## Example

User: "Use the clarify skill. I want to add rate limiting to my Laravel API."

Round 1 — call `AskUserQuestion` with:

- **Scope** (`multiSelect: true`): which routes get limited? Options: *All routes
  (Recommended)*, *Auth endpoints only*, *Public API only*, *Specific controllers*.
- **Key**: what is a request counted against? Options: *Per-IP (Recommended)*, *Per-user
  (Sanctum)*, *Per-API-key*, *Mix of IP + user*.
- **Storage**: where does the counter live? Options: *Redis (Recommended) — you already
  use it for cache*, *Database*, *In-memory (single server only)*.
- **Limits**: do you have numbers in mind? Options: *Propose sensible defaults
  (Recommended)*, *I'll specify exact limits*.

If anything's still unclear after the answers, ask another round; otherwise proceed.
