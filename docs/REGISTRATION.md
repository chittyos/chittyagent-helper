# ChittyHelper Registration (Through ChittyRegister)

This documents the canonical registration path, starting at `get.chitty.cc`.

## `register` vs `registry` — these are different services

Frequently confused; the distinction is load-bearing:

- **ChittyRegister** (verb, Tier 1) — the **gatekeeper** at `register.chitty.cc`. You **submit** to it. Open submission (no token required for first-party services; proof-of-control on `/health` is the credential). Mints ChittyIDs, issues certs, propagates to Registry and Discovery on success.
- **ChittyRegistry** (noun, Tier 2) — the **directory** at `registry.chitty.cc`. You **read** from it. Read-only catalog of certified entries; will reject direct POSTs with `READ_ONLY` code pointing back to `register.chitty.cc`.
- **ChittyDiscovery** (Tier 3) — runtime mesh at `discovery.chitty.cc`. Resolves **live endpoints** of running services. Not for catalog reads; for "where is the canonical instance of service X right now."

Mnemonic: Register = verb (action of submitting). Registry = noun (the resulting list). Discovery = adverb (where, right now). All three share the prefix; all three are different services.

## Flow

- Start: `get.chitty.cc` — obtain client credentials (CHITTY_REGISTER_TOKEN) and onboarding instructions.
- Register: `register.chitty.cc` — submit the service payload to `/api/v1/register`.
- Verify: query compliance and bindings; publication proceeds to Registry and Discovery.

## Prerequisites
- CHITTY_REGISTER_TOKEN (from get.chitty.cc)
- Payload file: `chittyfoundation/helper/registry/chittyhelper.registration.json`

## Submit Registration

```bash
export CHITTY_REGISTER_TOKEN="<your token from get.chitty.cc>"
# Production host
chittyfoundation/helper/registry/register-chittyhelper.sh
# Workers fallback (if DNS not available locally)
chittyfoundation/helper/registry/register-chittyhelper.sh chittyregister-production.chitty.workers.dev
```

## Check Compliance

```bash
curl -sS https://register.chitty.cc/api/v1/compliance/chittyhelper | jq .
```

Notes:
- ChittyRegister validates required fields and may mint a ChittyID and certificate before binding to Registry and Discovery.
- ChittyHelper remains Class III (informational); endpoints are read-only.

