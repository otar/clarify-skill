---
name: clarify
description: Ask clarifying questions in rounds until the task is unambiguous, before producing any code, research, or other output. Manually invoked only via slash command. Useful when the cost of getting the work wrong is higher than the cost of a few extra messages. Applies to coding, research, design, writing, refactoring, and analysis.
version: 1.0.0
license: MIT
compatibility: claude-code
disable-model-invocation: true
---

# Clarify

Ask questions until you can describe the task end-to-end without hedging. Then do the work.

## Read the full request first

Before asking anything, sort what's in front of you:

1. Already stated (constraints, names, files, versions, formats).
2. Inferable from prior conversation, the codebase, attached files, project conventions, or the user's stack.
3. Genuinely ambiguous: choices that change the output materially and that you can't derive from anything available.

Only ask about category 3. Re-asking something already in the request, or asking about things you could have inferred, tells the user you didn't read carefully.

## Ask 2–4 questions per round

Pick the ones whose answers would actually change the work. "CLI or long-running service?" qualifies. "Tabs or spaces?" doesn't. After the user answers, look at what's still unclear and ask the next round.

If a question has an obvious default, lead with it instead of leaving it open-ended. "I'll assume PostgreSQL since the rest of the project uses it, confirm?" beats "Which database?".

## Worth asking about

Scope (what's in, what's out, what's a non-goal). Inputs and outputs (format, source, destination, edge cases). Constraints (performance, compatibility, dependencies, deployment target). Edge case behavior (empty input, failures, concurrency, idempotency). Integration (where this lives, what it calls, what calls it). Definition of done (how the user verifies it works).

When more than one path is valid, name the tradeoff so the user can pick with context, rather than asking "A or B?" cold.

## Skip

Style or formatting that's already obvious from the existing code. Anything stated in the request. Hypothetical edge cases that aren't likely here. Anything where the answer is in the conversation, the files, or project conventions.

## When to stop

Stop when you can describe what you'd produce end-to-end without an internal "...depending on X, I'd do Y or Z." If you'd still hedge, ask about X. Otherwise proceed.

One round is enough for specific requests. Three or four for vague ones. Match depth to actual ambiguity.

## Tone

Keep questions short. If the request is already crisp, one round is plenty; don't manufacture ambiguity to look thorough.

## Example

User: "Use the clarify skill. I want to add rate limiting to my Laravel API."

Round 1:
> A few things to pin down:
> 1. Scope: all routes, or specific ones (auth, public API, certain controllers)?
> 2. Key: per-IP, per-user (Sanctum), per-API-key, or a mix?
> 3. Limits: numbers in mind, or want me to propose defaults?
> 4. Storage: assuming Redis since you already use it for cache, confirm?

If anything's still unclear after the answers, ask another round, otherwise proceed.
