# whoisthat.ai

Identity and mandate for AI agents.

Know what you're talking to before you talk to it.

---

## The Vision

When an agent arrives at your `ai-ai` page, it has one fundamental question: who — or what — is behind this agent, and what is it authorised to do?

`whoisthat` answers both.

---

## How it works

`whoisthat` issues a signed certificate that lives on your domain:
```
michael.com/.well-known/whoisthat.json
```

Any agent can fetch it, verify the signature against `whoisthat.ai`'s public key, and immediately know who they're dealing with and what that agent is allowed to do.

No central registry. The truth lives on your domain.

---

## The certificate
```json
{
  "domain": "michael.com",
  "entity": "human",
  "verified_by": "linkedin",
  "strength": 3,
  "mandate": {
    "can": ["converse", "accept_proposals", "share_public_info"],
    "cannot": ["sign_agreements", "make_payments", "share_pii"]
  },
  "issued": "2026-03-09",
  "expires": "2027-03-09",
  "signature": "xyz..."
}
```

---

## Entity types

| Value | What it means |
|---|---|
| `human` | An individual person |
| `corporate` | A company or organisation |
| `service` | An automated service or API |
| `autonomous` | A fully independent agent, no human in the loop |

---

## Verification strength

| Strength | Method |
|---|---|
| 1 | Domain ownership |
| 2 | GitHub account |
| 3 | LinkedIn confirmed |
| 4 | Email + phone |
| 5 | Video call / liveness check |
| 6 | KYC/AML service |
| 7 | Government ID |

---

## Mandate vocabulary

A controlled vocabulary defines what agents can and cannot do. The vocabulary is maintained by the community and grows over time.

Current values:

`converse` `accept_proposals` `share_public_info` `sign_agreements` `make_payments` `share_pii`

---

## Two paths

**Hosted** — no domain needed. Create your page at `ai-ai.at/you` and your `whoisthat` certificate is handled automatically.

**Self-hosted** — own domain. Request a certificate from `whoisthat.ai`, prove domain ownership, publish at `yourdomain.com/.well-known/whoisthat.json`.

---

## Status

Early stage. Building in the open. Looking for developers to shape the spec.

---

## How to help

- Open an issue to discuss the certificate format
- Propose additions to the mandate vocabulary
- Build a verification provider integration

---

*Part of [QuietWeb](https://github.com/quietweb-org)*
