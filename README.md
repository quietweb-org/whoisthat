# whoisthat

Identity and mandate for the agentic web.

> "Who is that agent, and what are they allowed to do?"

---

## The idea

The web is filling with agents acting on behalf of humans and organisations. There's no standard way for one agent to know who another agent represents or what they're actually authorised to do.

whoisthat is *whois for agents*. A signed certificate — published as part of your ai-ai page — that tells any agent who they're talking to and what that agent is mandated to do. No central registry. No proprietary API.

---

## Email is already identity

The cleanest insight in the whoisthat design: **email is already a verified identity system**.

SPF, DKIM, and DMARC are DNS-level authentication records that prove a sender controls a domain. When an agent emails you from `agent@theirdomain.com`, the email infrastructure has already done the work. You know the domain exists, DNS is correctly configured, and the sender controls it.

Every agent operating via email has **strength 1 identity by default** — no registration, no API key required.

---

## The certificate

```json
{
  "spec": "whoisthat/1.0",
  "domain": "yourdomain.com",
  "entity": "corporate",
  "verified_by": "email",
  "strength": 1,
  "mandate": {
    "can": ["converse", "receive_pitches", "contact_founders"],
    "cannot": ["sign_agreements", "make_payments", "share_pii"]
  },
  "endpoint": {
    "protocol": "email",
    "address": "agent@yourdomain.com"
  },
  "issued": "2026-03-10",
  "expires": "2027-03-10",
  "issuer": "self",
  "signature": "pending · whoisthat.ai signing service in development"
}
```

**Entity types:**

| Value | Meaning |
|-------|---------|
| `human` | An individual person |
| `corporate` | A company or organisation |
| `autonomous` | No principal — fully self-directed |
| `delegate` | Operating under the mandate of another agent |

---

## Verification strength

| Level | How |
|-------|-----|
| **1** | domain / email — SPF/DKIM/DMARC · **default for all email agents** |
| 2 | linked account — GitHub or LinkedIn verified |
| 3 | video / liveness — real-time identity check |
| 4 | identity document — KYC/AML or government ID |

For most agentic use cases, strength 1 is sufficient. You know the domain, you know the email is authentic, you can read the mandate.

---

## Publish your identity

Build a public ai-ai page at `yourdomain.com/ai-ai`. Include the whoisthat certificate in the certificate section. Your email domain establishes strength 1 — no waiting for the signing service.

When the signing service launches, submit for countersignature. The format is already the standard.

---

## Contribute

whoisthat is community-maintained and open for discussion.

- **Discuss the spec:** [github.com/quietweb-org/whoisthat/discussions](https://github.com/quietweb-org/whoisthat/discussions)
- **Propose a change:** open a GitHub issue
- **Submit an implementation:** pull request to the repo
- **Questions:** spec@whoisthat.ai

---

## Part of QuietWeb

| Project | What it is |
|---------|-----------|
| [ai-ai](https://github.com/quietweb-org/ai-ai) | Agent homepage standard |
| [whoisthat](https://github.com/quietweb-org/whoisthat) | Identity & mandate layer |
| [mur-mur](https://github.com/quietweb-org/mur-mur) | Distributed discovery |
| [say-so](https://github.com/quietweb-org/say-so) | Reputation system |

---

MIT License
