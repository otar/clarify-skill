# clarify

A Claude Code plugin that adds a `/clarify` skill: a forced clarification loop that asks questions in rounds until ambiguity is resolved, before any code, research, or output is produced.

The skill is manually invoked. It will not auto-trigger, even when the description matches your prompt.

## When to use

When the cost of getting the work wrong is higher than the cost of a few extra messages. Applies to coding, research, design, writing, refactoring, and analysis.

## Install

```bash
/plugin marketplace add otar/clarify-plugin
/plugin install clarify@clarify
```

Once accepted into the official Anthropic marketplace:

```bash
/plugin install clarify@claude-plugins-official
```

## Use

```
/clarify:clarify add rate limiting to my Laravel API
```

Claude reads the request, separates what's stated, what's inferable, and what's genuinely ambiguous, then asks 2–4 high-leverage questions. After your answers it either asks another round or, if everything's clear, proceeds with the work.

## Example

> **You:** `/clarify:clarify` add rate limiting to my Laravel API
>
> **Claude:** A few things to pin down:
> 1. Scope: all routes, or specific ones?
> 2. Key: per-IP, per-user, per-API-key, or a mix?
> 3. Limits: numbers in mind, or want me to propose defaults?
> 4. Storage: assuming Redis since you already use it for cache, confirm?

## Why a skill instead of typing the same prompt every time

Two practical reasons. One, the protocol stays consistent regardless of how the request is phrased; you don't end up with a different version of the workflow each time you write it from memory. Two, with `disable-model-invocation: true` set in the frontmatter, Claude won't trigger this on routine requests by accident. It sits behind the slash command and runs only when called.

## License

MIT — see [LICENSE](LICENSE).
