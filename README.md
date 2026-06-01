# clarify

A Claude Code plugin that adds a `/clarify` skill. It keeps asking clarifying questions until your request is unambiguous, before any work starts.

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

Claude reads the request, works out what's actually unclear (versus what's stated or reasonable to assume), and asks 2–4 questions. Once you answer, it either follows up with another round or gets on with the work.

## Example

> **You:** `/clarify:clarify` add rate limiting to my Laravel API
>
> **Claude:** A few things to pin down:
> 1. Scope: all routes, or specific ones?
> 2. Key: per-IP, per-user, per-API-key, or a mix?
> 3. Limits: numbers in mind, or want me to propose defaults?
> 4. Storage: assuming Redis since you already use it for cache, confirm?

## Why a skill instead of typing the same prompt every time

A couple of reasons. The skill behaves the same way every time, so you don't end up with a slightly different workflow depending on how you phrased the prompt that day. And with `disable-model-invocation: true` in the frontmatter, Claude won't trigger it on routine requests by accident; it only runs when you call it.

## Privacy

The plugin runs locally and collects nothing. See [PRIVACY.md](PRIVACY.md).

## License

MIT. See [LICENSE](LICENSE).
