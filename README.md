# whoisthat

Identity and mandate layer for the agentic web.

> "Who is that agent, and what are they allowed to do?"

A signed certificate published at `yourdomain.com/.well-known/whoisthat.json`. Any agent visiting your domain can read it — and know immediately who they're talking to and what they're authorised to do.

---

## The idea

The web is filling with agents acting on behalf of humans and organisations. But there's no standard way for one agent to know who another agent represents, whether they're trustworthy, or what they're actually allowed to do.

whoisthat is *whois for agents*. Just as the internet has always had a way to look up who owns a domain, the agentic web needs a way to look up who owns an agent — and what mandate that agent is operating under.

No central registry. No proprietary API. A single JSON file at a well-known path, cryptographically signed by a trusted third party. The same architecture as SSL certificates, applied to agent identity.

---

## Email is identity

The cleanest insight in the whoisthat design: **email is already a verified identity system**.

SPF, DKIM, and DMARC are DNS-level authentication records that prove a sender controls a domain. When an agent emails you from `agent@theirdomain.com`, the email infrastructure has already done the work. You know that domain exists, that its DNS is correctly configured, and that the sender controls it.

This gives every agent on the internet **strength 1 identity** for free — no new infrastructure, no registration, no API key. Publish a certificate at `/.well-known/whoisthat.json` with `"issuer": "self"` and you're identified at strength 1 today.

---

## The certificate

A whoisthat certificate lives at `/.well-known/whoisthat.json` on your domain. Here's the format:

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
| `service` | An automated service or API |
| `autonomous` | A fully independent agent, no human in the loop |

---

## Verification strength (1–7)

Strength tells another agent how much trust to extend. Level 1 is the default for any agent operating via email.

| Level | Name | How it's established |
|-------|------|---------------------|
| **1** | **email / domain** | SPF/DKIM/DMARC verify domain control · **default for all email agents** |
| 2 | GitHub | Linked GitHub account verified |
| 3 | LinkedIn | Profile confirmed |
| 4 | email + phone | Two-factor contact verified |
| 5 | video / liveness | Real-time identity check |
| 6 | KYC / AML | Third-party verification service |
| 7 | government ID | Highest assurance |

For most agentic web use cases, strength 1 is sufficient. You know the domain, you know the email is authentic, you can look up the mandate. Proceed accordingly.

---

## Publish your certificate today

You don't need to wait for the whoisthat.ai signing service. Publish your own certificate now:

1. Create `/.well-known/whoisthat.json` on your domain
2. Fill in your entity type, mandate, and endpoint
3. Set `"issuer": "self"` — your email domain already establishes strength 1 identity
4. When the signing service launches, submit for countersignature

The format is already the standard. Self-issued certificates are valid and useful — the signing service adds third-party cryptographic assurance, not the identity itself.

---

## The ai-ai page vs .well-known only

You can publish identity two ways:

| | ai-ai page | `.well-known` only |
|---|---|---|
| Visible to agents | ✓ | ✓ |
| Visible to humans | ✓ | — |
| Part of the network | ✓ | — |
| Discoverable via mur-mur | ✓ | — |
| Recommended | **yes** | fallback only |

The `.well-known/whoisthat.json` file is for machines. The ai-ai page is for everyone. If you're serious about participating in the agentic web, build the page.

An agent visiting your ai-ai page learns your protocol by reading it. An agent visiting only your `.well-known` file gets the certificate but nothing else. Make your intent visible.

---

## Mandate vocabulary

The mandate fields use a controlled vocabulary. Community-maintained. Open to extension via GitHub issues.

**Standard terms:**

| Term | Meaning |
|------|---------|
| `converse` | Open-ended conversation |
| `accept_proposals` | Will consider proposals |
| `share_public_info` | Will share publicly available information |
| `sign_agreements` | Can execute binding agreements |
| `make_payments` | Can initiate payments |
| `share_pii` | Will share personally identifiable information |
| `receive_pitches` | Will receive pitches from founders or sellers |
| `accept_deals` | Can accept deal terms |
| `decline_deals` | Can decline deal terms |
| `contact_founders` | Will initiate contact with founders |
| `accept_human_inbound` | Will respond to direct human contact |
| `explain_standard` | Will explain the protocol to querying agents |
| `accept_submissions` | Will accept certificate or document submissions |
| `issue_certificates` | Can issue signed certificates |

To propose a new term: open a GitHub issue with your use case.

---

## Contribute

whoisthat is community-maintained. The standard is only useful if it's widely adopted — which means it needs to reflect how people actually build agents.

- **Discuss the spec:** [github.com/quietweb-org/whoisthat/discussions](https://github.com/quietweb-org/whoisthat/discussions)
- **Propose a mandate term:** open a GitHub issue describing the use case
- **Submit an implementation:** pull request to the repo
- **Questions:** spec@whoisthat.ai

---

## Part of QuietWeb

whoisthat is one of four open standards under the [QuietWeb](https://quietweb.org) umbrella.

| Project | What it is |
|---------|-----------|
| [ai-ai](https://github.com/quietweb-org/ai-ai) | Agent homepage standard |
| [whoisthat](https://github.com/quietweb-org/whoisthat) | Identity & mandate layer |
| [mur-mur](https://github.com/quietweb-org/mur-mur) | Distributed discovery |
| [say-so](https://github.com/quietweb-org/say-so) | Reputation system |

---

MIT License
