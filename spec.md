# Open Snow Discovery Standard — v0.1

A vertical profile of [ARD](https://agenticresourcediscovery.org/) /
[ai-catalog](https://github.com/Agent-Card/ai-catalog) for ski & snow agentic
resources, with a verification requirement. Status: Draft. Built on ai-catalog
`specVersion` 1.0.

## 1. Scope

Applies to publishers of ski/snow **agentic resources** — MCP servers, HTTP/OpenAPI
APIs, A2A agents, datasets, skills, or nested catalogs — that want to be discovered
and trusted by AI agents.

## 2. Publish (required)

Serve a static manifest at:

```
https://<domain>/.well-known/ai-catalog.json
```

Requirements:
- HTTPS only; `Content-Type: application/json`; `Access-Control-Allow-Origin: *`.
- `specVersion: "1.0"`, a `host` object, and an `entries[]` array.
- Each entry: a domain-anchored URN `identifier` (`urn:ai:<domain>:<namespace>:<name>`),
  a `type` media type, a `url`, a `description`, and **2–5 `representativeQueries`**
  written the way a skier would actually ask.

## 3. Verify (required for "Verified")

Add a `trustManifest` to the host and to every entry:

```json
"trustManifest": {
  "identity": "did:web:<domain>",
  "identityType": "did",
  "signature": "<detached JWS, compact>"
}
```

- **Identity:** `did:web:<domain>`, resolvable at `https://<domain>/.well-known/did.json`,
  whose `verificationMethod` publishes an Ed25519 `publicKeyJwk`.
- **Signature:** a **detached JWS** (RFC 7515, `alg: EdDSA`) over the **entry object**
  canonicalized with **JCS** (RFC 8785), with only `trustManifest.signature` removed.
  This binds the signature to the entry's `identifier`, `type`, `url`, capabilities, and
  identity — so any tamper invalidates it (addresses ai-catalog ADR-0009 substitution).

### Verification procedure
1. Fetch the catalog over HTTPS from the expected domain.
2. For each entry: remove `trustManifest.signature`, JCS-canonicalize the entry, recompute
   the JWS signing input, and verify against the key resolved from `trustManifest.identity`.
3. Reject entries that fail.

## 4. Conformance levels

| Level | Requirement |
|---|---|
| **Listed** | Valid `ai-catalog.json` per §2. |
| **Verified** | §2 + §3: signed entries that validate against the published `did:web` key. |

Registries MAY index Verified-only and MUST rank Verified above Listed.

## 5. Federate

Submit your domain to a conforming registry (ARD `POST /search`). The registry crawls
the catalog, verifies signatures, and federates results. Discovery is permissionless:
any conforming registry may index any conforming publisher; no central gatekeeper.

Reference registry: `POST https://www.snowsure.ai/search`.

## 6. Reference implementations

SnowSure, SnowData, LUXSKI, Afore, Ski Limone — five independent `did:web` publishers,
44 resources indexed, 41 verified.

## Changelog
- v0.1 (2026-06) — initial draft.
