# The Bar Test 🍺

**A universal code review checklist for input resilience — inspired by the most famous QA meme in software engineering.**

> *"A QA engineer walks into a bar. Orders a beer. Orders 0 beers. Orders 99999999999 beers. Orders a lizard. Orders -1 beers. Orders a ueicbksjdhd. First real customer walks in and asks where the bathroom is. The bar bursts into flames, killing everyone."*
>
> — [Brenan Keller](https://twitter.com/brenankeller/status/1068615953989087232), 2018

Every developer has lived this. Your code passes every test you can think of, then a real user does something completely normal and everything catches fire.

**The Bar Test** turns that pain into a method: 7 scenarios that every input boundary in your code should survive before hitting production.

## The 7 Scenarios

Apply to every code path that receives external input: API endpoints, form fields, URL params, file uploads, webhook payloads, CLI arguments, inter-service messages.

| # | Scenario | The Bar Equivalent | What to Check |
|---|----------|--------------------|---------------|
| 1 | **Happy Path** | Orders a beer | Does it work with expected input? |
| 2 | **Nothing** | Orders 0 beers | Empty strings, null, undefined, zero, missing keys |
| 3 | **Too Much** | Orders 99999999999 beers | MAX_INT, 100k-char strings, huge payloads |
| 4 | **Wrong Type** | Orders a lizard | String where number expected, array where object expected |
| 5 | **Negative / Inverted** | Orders -1 beers | Negative numbers, reversed date ranges, inverted logic |
| 6 | **Malicious / Garbage** | Orders a ueicbksjdhd | SQL injection, XSS, path traversal, unicode tricks |
| 7 | **The Unforeseen Normal** | Asks where the bathroom is | Double-submit, stale tabs, expired sessions, mid-operation disconnect |

## Quick Reference

For each input boundary in your code:

| # | Question | If No |
|---|----------|-------|
| 1 | Does the happy path work correctly? | Fix the logic |
| 2 | Is absence handled without crashing? | Add null/empty checks |
| 3 | Are limits enforced before processing? | Add max length/size validation |
| 4 | Is type coercion explicit, not implicit? | Add type validation at the boundary |
| 5 | Are impossible values rejected early? | Add range/logic validation |
| 6 | Is input sanitized before use? | Add escaping/parameterization |
| 7 | Is the operation idempotent or protected against replay? | Add guards (idempotency keys, optimistic locking, debounce) |

## The Rule

```
Code that only handles #1 is a prototype.
Code that handles #1-#6 is production-ready.
Code that also considers #7 is resilient.
```

## Usage

### In code reviews
> "This endpoint fails Bar Test #2 — what happens when `quantity` is `null`?"

### In PR templates
Add to your PR checklist:
```markdown
- [ ] Passes The Bar Test (scenarios 1-7)
```

### As an AI code review prompt
Feed [THE_BAR_TEST.md](./THE_BAR_TEST.md) to your AI code review agent as system instructions.

### In your repo
```bash
curl -o THE_BAR_TEST.md https://raw.githubusercontent.com/Nikoxx99/the-bar-test/main/THE_BAR_TEST.md
```

## Detailed Checklist

See [THE_BAR_TEST.md](./THE_BAR_TEST.md) for the full expanded checklist with specific examples for each scenario.

## Contributing

Found a scenario #7 that burned your production? Open an issue or PR. The best additions come from real-world war stories.

## License

[MIT](./LICENSE) — Use it, share it, put it in your PRs.
