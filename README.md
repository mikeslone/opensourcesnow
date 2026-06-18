# Open Snow Discovery Standard

An open, vendor-neutral standard for making **ski & snow** agentic resources
discoverable — and verifiable — by AI agents. Built on
[Agentic Resource Discovery (ARD)](https://agenticresourcediscovery.org/) and the
[ai-catalog](https://github.com/Agent-Card/ai-catalog) standard.

**Live:** https://opensourcesnow.com

## What this is

A vertical profile of ARD for ski & snow, plus a verification requirement:

1. **Publish** a static `ai-catalog.json` at `/.well-known/` (HTTPS, `application/json`, CORS-open).
2. **Verify** with a `did:web` identity + Ed25519 entry-bound detached JWS (RFC 7515 over RFC 8785 JCS), public key at `/.well-known/did.json`.
3. **Federate** by submitting your domain to a conforming discovery registry.

Two conformance levels: **Listed** (valid catalog) and **Verified** (signed + DID-resolvable).

- Human-readable standard: [`index.html`](./index.html) → https://opensourcesnow.com
- Developer spec: [`spec.md`](./spec.md)
- Machine-readable profile: [`spec.json`](./spec.json)

## Reference implementations

Five independent publishers serving signed, verified catalogs today: SnowSure,
SnowData, LUXSKI, Afore, Ski Limone. Reference registry (ARD `POST /search`):
`https://www.snowsure.ai/search`.

## Join

Run a ski/snow MCP server, API, agent, or dataset? See https://www.snowdata.ai/network.

## Deploy

Static site — no build step. Deploy the repo root to any static host (Vercel:
"Other" framework preset, output dir = `.`), point `opensourcesnow.com` at it.

## License

Spec text: CC BY 4.0. Code samples: MIT. See [`LICENSE`](./LICENSE).
Adoption is permissionless — this standard does not gatekeep discovery.
