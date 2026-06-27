# clarify

A Claude Code plugin that adds a `/clarify` skill. It interrogates your request with structured, clickable questions until it's unambiguous, before any work starts.

Questions arrive through the `AskUserQuestion` tool: 2–4 options each, with a recommended default up front, so you usually answer in a tap instead of typing. The skill leans toward asking — it surfaces borderline ambiguity rather than guessing.

The skill is manually invoked. It will not auto-trigger, even when the description matches your prompt.

## When to use

When the cost of getting the work wrong is higher than the cost of a few extra messages. Useful for coding, writing, or any task where the brief is fuzzy.

## Install

```bash
/plugin marketplace add otar/clarify-plugin
/plugin install clarify@otar
```

## Use

```
/clarify:clarify add rate limiting to my Laravel API
```

Claude reads the request, works out what's actually unclear (and surfaces anything it would otherwise have to assume), then asks up to four structured questions at once. Once you answer, it either follows up with another round or gets on with the work.

## Example

> **You:** `/clarify:clarify` add rate limiting to my Laravel API
>
> **Claude** opens a question prompt with clickable options:
> - **Scope** (pick any): All routes *(recommended)* · Auth only · Public API only · Specific controllers
> - **Key:** Per-IP *(recommended)* · Per-user (Sanctum) · Per-API-key · Mix
> - **Storage:** Redis *(recommended — you already use it for cache)* · Database · In-memory
> - **Limits:** Propose defaults *(recommended)* · I'll specify
>
> Every question has an "Other" option for anything that doesn't fit.

## Why a skill instead of typing the same prompt every time

A couple of reasons. The skill behaves the same way every time — same structured prompts, same bias toward asking — so you don't end up with a slightly different workflow depending on how you phrased the prompt that day. And with `disable-model-invocation: true` in the frontmatter, Claude won't trigger it on routine requests by accident; it only runs when you call it.

## Privacy

The plugin runs locally and collects nothing. See [PRIVACY.md](PRIVACY.md).

## License

MIT. See [LICENSE](LICENSE).
