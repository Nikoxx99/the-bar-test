# The Bar Test

> Inspired by [Brenan Keller's tweet](https://twitter.com/brenankeller/status/1068615953989087232) (2018):
> *"A QA engineer walks into a bar. Orders a beer. Orders 0 beers. Orders 99999999999 beers. Orders a lizard. Orders -1 beers. Orders a ueicbksjdhd. First real customer walks in and asks where the bathroom is. The bar bursts into flames, killing everyone."*
>
> **Objective:** A universal, language-agnostic checklist for reviewing any code that handles external input. Apply it to PRs, audits, or feed it to an AI code review agent.

---

## The 7 Scenarios

Review every code path that receives external input (API endpoints, form fields, URL params, file uploads, webhook payloads, CLI arguments, inter-service messages).

### 1. Happy Path
The input is exactly what the developer expected.
- Does it produce the correct result?
- Are success responses/states properly returned?

### 2. Nothing
The input is empty, null, undefined, zero, or absent.
- Empty string `""`
- `null` / `undefined` / missing key
- `0` where a positive number is expected
- Empty array `[]` or object `{}`
- Omitted optional fields

### 3. Too Much
The input is technically valid but absurdly large.
- Numbers near MAX_INT or MAX_FLOAT
- Strings with 100k+ characters
- Arrays with 10k+ elements
- Deeply nested objects (100+ levels)
- File uploads at the size limit (or beyond)

### 4. Wrong Type
The input is a completely different type than expected.
- String where number is expected (and vice versa)
- Array where object is expected
- Boolean where string is expected
- Object where primitive is expected

### 5. Negative / Inverted
The input inverts the expected logic.
- Negative numbers where only positive makes sense
- End date before start date
- Reversed min/max ranges
- Negative quantities, prices, or counts

### 6. Malicious / Garbage
The input is intentionally crafted to break things.
- SQL injection: `'; DROP TABLE users;--`
- XSS: `<script>alert(1)</script>`
- Path traversal: `../../etc/passwd`
- Unicode edge cases: zero-width chars, RTL overrides
- Random bytes / undecodable strings
- JSON with `__proto__` or `constructor` keys

### 7. The Unforeseen Normal
The action is completely legitimate but nobody coded for it.
- Submitting the same form twice (double click, network retry)
- Using the system after session/token expiry
- Performing actions in an unexpected order
- Concurrent edits to the same resource
- Copy-pasting data with hidden characters (tabs, BOM, CRLF)
- Acting on stale data (old tab, cached page)
- Disconnecting mid-operation (network drop, browser close)

## How to Apply

For each input boundary in the code under review, ask:

| # | Question | If No |
|---|----------|-------|
| 1 | Does the happy path work correctly? | Fix the logic |
| 2 | Is absence handled without crashing? | Add null/empty checks |
| 3 | Are limits enforced before processing? | Add max length/size validation |
| 4 | Is type coercion explicit, not implicit? | Add type validation at the boundary |
| 5 | Are impossible values rejected early? | Add range/logic validation |
| 6 | Is input sanitized before use? | Add escaping/parameterization |
| 7 | Is the operation idempotent or protected against replay? | Add guards (idempotency keys, optimistic locking, debounce) |

## Rule

Code that only handles #1 is a prototype.
Code that handles #1-#6 is production-ready.
Code that also considers #7 is resilient.
