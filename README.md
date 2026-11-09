# lightning-gifts-skill

An agent skill for creating and funding bitcoin gifts with the
[lightning.gifts](https://lightning.gifts) HTTP API — written and maintained by the operator of the
service itself.

```
npx skills add wahidmy/lightning-gifts-skill
```

Installs one skill, `lightning-gifts`, into whichever agent directories you pick during the prompt.

## What it covers

The giver half of a gift, end to end:

- sign in with the email link, or use a per-user API key;
- price a gift in sats, with the fee and the per-deployment ceiling;
- create the gift, pay its BOLT11 funding invoice, and watch it flip `UNFUNDED` → `UNSEEN`;
- mint the one-time claim link (`?ct=`) that is the actual gift;
- reclaim an expired gift and pay yourself out.

The recipient's claim and withdrawal are documented only as far as an agent needs to explain them at
handoff. Operator and admin routes are deliberately out of scope.

## Layout

```
skills/lightning-gifts/SKILL.md               the procedure, ~200 lines
skills/lightning-gifts/references/endpoints.md  route tables, error bodies, worked curl
skills/lightning-gifts/references/pitfalls.md   twelve traps, each with why and the fix
```

Progressive disclosure: `SKILL.md` is what an agent reads to do the job; the two references are
loaded only when a step points at them.

## How this stays true

The service's own `/api-docs.md` is generated from the running source and byte-compared by the test
suite, so it — not this repository — is the authority on payloads and field names. This skill carries
the procedure, the ordering, and the traps that cost people gifts; it deliberately refuses to
hard-code operational numbers (fees, ceilings, invoice caps), because those are per-deployment
settings that change without a code change.

If the two disagree, the service wins and this repository has a bug. Issues are welcome.

## Licence

MIT — see [LICENSE](LICENSE). The lightning.gifts service itself is a fork of
[Lightsats](https://github.com/lightsats/lightsats) (MIT) with modifications under the Blue Oak Model
License 1.0.0.
