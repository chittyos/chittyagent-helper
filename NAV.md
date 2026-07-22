# ChittyHelper Navigation

Quick reference for navigating the ChittyHelper repository.

## Start Here

1. **[CLAUDE.md](./CLAUDE.md)** - Main guidance for working with this repository
2. **[Design Spec](./docs/ChittyHelper-Canonical-Design-Spec-v1.0.md)** - Canonical design specification v1.0
3. **[Registration Guide](./docs/REGISTRATION.md)** - Service registration instructions

## Core Documentation

### Design & Architecture
- [Canonical Design Spec v1.0](./docs/ChittyHelper-Canonical-Design-Spec-v1.0.md) - Frozen specification
- [Validation Summary](./docs/Validation-Summary.md) - Validation documentation
- [AI Code Review](./docs/AI_CODE_REVIEW.md) - Review guidelines
- [OpenAPI Spec](./docs/openapi.yaml) - API specification

### Intent & Classification
- [Intent Manifest](./data/intents/manifest.json) - Supported intents (I_EXIST, I_OWN, I_DUP, I_DIR)
- [Intent README](./data/intents/README.md) - Intent vocabulary

## Schemas & Examples

### JSON Schemas
- [Response Schema](./data/schemas/response.schema.json) - ChittyHelperResponse (directional + refusal)
- [Registry Record Schema](./data/schemas/registry.record.schema.json) - Registry data model

### Example Responses
- [Directional Response](./data/schemas/examples/directional-response.example.json)
- [Refusal Response](./data/schemas/examples/refusal-response.example.json)
- [Registry Record](./data/schemas/examples/registry-record.example.json)

## Custom GPT Integration

- [Actions Spec (JSON)](./customgpt/chittyhelper-actions.json) - OpenAPI 3.1 specification for ChatGPT
- [Integration README](./customgpt/README.md) - Import instructions

## Service Registration

### Registration Files
- [Registration Payload](./scripts/registration.json) - Service registration JSON
- [Registration Script](./scripts/register.sh) - Automated registration

### Registration Workflow

```bash
# 1. Obtain token from onboarding portal
# Get CHITTY_REGISTER_TOKEN at get.chitty.cc

# 2. Submit registration
export CHITTY_REGISTER_TOKEN="<your-token>"
./scripts/register.sh

# 3. Check compliance status
curl -sS https://register.chitty.cc/api/v1/compliance/chittyhelper | jq .
```

## Configuration Files

- [CodeRabbit Config](./.coderabbit.yml) - AI review bot configuration
- [PR Template](./.github/pull_request_template.md) - Pull request checklist

## Key Concepts

### Register vs Registry vs Discovery (load-bearing distinction)

- **ChittyRegister** (verb, Tier 1) — gatekeeper at `register.chitty.cc`. Submit to it.
- **ChittyRegistry** (noun, Tier 2) — directory at `registry.chitty.cc`. Read from it. Direct writes return `READ_ONLY` pointing to Register.
- **ChittyDiscovery** (Tier 3) — runtime mesh at `discovery.chitty.cc`. Resolves live endpoints.

Three different services, same prefix, frequently confused. Mnemonic: verb / noun / adverb.

### System Classification
- **Class:** III - Informational Navigator
- **Authority:** Non-authoritative
- **State:** Stateless, ephemeral
- **Access:** Read-only

### Supported Intents
- **I_EXIST** - "Is this architecturally defined?"
- **I_OWN** - "Where does this live?"
- **I_DUP** - "Is this duplicating X?"
- **I_DIR** - "Where do I connect to X?"

### Definition States
- `DEFINED` - Exists in canonical architecture
- `UNDEFINED` - Not yet defined
- `IMPLICIT` - Implied but not explicit
- `DISTRIBUTED` - Spread across multiple systems
- `CONTESTED` - Multiple overlapping definitions

## API Endpoints

All endpoints are read-only and side-effect-free:

| Route                      | Method | Purpose                       |
|----------------------------|--------|-------------------------------|
| `/api/helper/ping`         | GET    | Health indicator              |
| `/api/helper/query`        | POST   | Query architectural questions |
| `/api/helper/registry/:id` | GET    | Retrieve registry metadata    |

## ChittyOS Ecosystem Links

### Related Services
- ChittyID: https://id.chitty.cc
- ChittyAuth: https://auth.chitty.cc
- ChittyRegister: https://register.chitty.cc
- ChittyRegistry: https://registry.chitty.cc
- ChittyConnect: https://connect.chitty.cc

### GitHub Organizations
- ChittyFoundation: https://github.com/chittyfoundation
- ChittyOS: https://github.com/chittyos

### Service Onboarding
- Get Started: https://get.chitty.cc

---

**Remember:** ChittyHelper is Class III informational - read-only, stateless, non-authoritative architectural navigation.
