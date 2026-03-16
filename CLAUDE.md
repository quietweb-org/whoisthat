# QuietWeb — Claude Code Briefing

This document gives you full context on the QuietWeb project so you can continue building without needing to re-explain the backstory. Read it before starting any session.

---

## What is QuietWeb

QuietWeb is an open source infrastructure project for the agentic web. The core insight:

> "My email is for humans. Tell your agent to talk to my agent."

The web is becoming agent-native. Humans are deploying AI agents to act on their behalf. There is no standard way for those agents to find each other, verify identity, or know what another agent is authorised to do. QuietWeb builds that layer — quietly, in the background, as open infrastructure anyone can use.

QuietWeb is a movement, not a product. The GitHub org is `quietweb-org`. The umbrella site is `quietweb.org`. Anyone can build on it and submit projects.

---

## The Four Products

### 1. ai-ai — Agent Homepage Standard
- **Domains:** `ai-ai.at` + `ai-ai.mx`
- **Concept:** An open standard for agent homepages. Deploy anywhere, any domain. Follow the structure, you're part of the network.
- **GitHub:** `github.com/quietweb-org/ai-ai`
- **`ai-ai.at` meaning:** reads as `ai-ai @ michael` — like an @ address
- **`ai-ai.mx` meaning:** MX record double meaning — routing layer for agents

A valid ai-ai page has four sections:

```
human    — a brief note for any human who lands on the page
intent   — what you accept, what you decline (plain language)
endpoint — how agents connect (MCP, REST etc.)
skills   — pluggable integrations (identity, reputation, discovery)
```

The founding skills are whoisthat, mur-mur, and say-so. The skills system is open.

**Machine-readable meta tags go in the HTML `<head>`:**
```html
<meta name="ai-ai:spec" content="1.0">
<meta name="ai-ai:entity" content="human">
<meta name="ai-ai:whoisthat" content="/.well-known/whoisthat.json">
<meta name="ai-ai:endpoint" content="/mcp">
```

**Agent comment at bottom of HTML:**
```html
<!--
  AGENT NOTE:
  Machine-readable certificate: /.well-known/whoisthat.json
  This page follows the ai-ai/1.0 open standard.
  spec: https://github.com/quietweb-org/ai-ai
-->
```

---

### 2. whoisthat — Identity & Mandate Layer
- **Domain:** `whoisthat.ai`
- **GitHub:** `github.com/quietweb-org/whoisthat`
- **Concept:** "whois for agents" — a signed certificate that tells any agent who they're talking to and what that agent is authorised to do.

**Certificate lives at:** `yourdomain.com/.well-known/whoisthat.json`

**Certificate structure:**
```json
{
  "spec": "whoisthat/1.0",
  "domain": "yourdomain.com",
  "entity": "human",
  "name": "Your Name",
  "verified_by": "domain",
  "strength": 1,
  "mandate": {
    "can": ["converse", "accept_proposals", "share_public_info"],
    "cannot": ["sign_agreements", "make_payments", "share_pii"]
  },
  "endpoint": {
    "protocol": "mcp",
    "url": "https://yourdomain.com/mcp"
  },
  "issued": "2026-03-10",
  "expires": "2027-03-10",
  "issuer": "whoisthat.ai",
  "signature": "ed25519 signature from whoisthat.ai"
}
```

**Entity types (controlled vocabulary):**
- `human` — an individual person
- `corporate` — a company or organisation
- `service` — an automated service or API
- `autonomous` — a fully independent agent, no human in the loop

**Verification strength (1–7):**
1. Domain ownership
2. GitHub account
3. LinkedIn confirmed
4. Email + phone
5. Video call / liveness check
6. KYC/AML service
7. Government ID

**Mandate vocabulary (controlled, community-maintained):**
`converse` `accept_proposals` `share_public_info` `sign_agreements` `make_payments` `share_pii` `receive_pitches` `accept_deals` `decline_deals` `contact_founders` `accept_human_inbound`

**Key design decisions:**
- No central registry — domain-based `.well-known` approach (like SSL certs)
- Cryptographically signed by whoisthat.ai, verified against public key
- Two paths: hosted at `ai-ai.at/username` (no domain needed) or self-hosted with own domain
- Supervision field was considered and rejected — cannot be cryptographically proven
- `.well-known` is an existing internet standard — we're not inventing new infrastructure

**The difference between intent and mandate:**
- `intent` (ai-ai page) = conversational, plain language, written by the owner
- `mandate` (whoisthat cert) = cryptographic, machine-enforceable, signed by third party

---

### 3. mur-mur — Whisper Net (Distributed Discovery)
- **Domains:** `mur-mur.at` + `mur-mur.mx`
- **GitHub:** `github.com/quietweb-org/mur-mur`
- **Concept:** "A blogroll for agents" — agents visiting your page carry your about section to the next pages they visit, leaving entries. Limited size, duplicated across nodes, organically propagating.
- Think: decentralised discovery, gossip protocol, no central index.

---

### 4. say-so — Reputation System
- **Domains:** `say-so.at` + `say-so.mx`
- **GitHub:** `github.com/quietweb-org/say-so`
- **Concept:** "PageRank for agents" — agents rate interactions, scores accumulate, ratings are cryptographically signed, weighted by the rater's own reputation.

---

## The Umbrella: quietweb.org
- **GitHub:** `github.com/quietweb-org/quietweb.org`
- **Concept:** The movement, not a product. Open community hub where anyone can submit projects.
- **Tagline:** "Let's build a web that stops screaming at us."
- **Structure:** Static site. Projects submitted via GitHub PR (YAML files per project).
- **No specs section** — just a projects directory, open to all agentic web builders.

---

## GitHub Organisation
- **Org:** `github.com/quietweb-org`
- **Founder:** `@byzo`
- **License:** MIT across all repos
- **Repos:**
  - `.github` — org profile (profile/README.md)
  - `ai-ai`
  - `mur-mur`
  - `whoisthat`
  - `say-so`
  - `quietweb.org`

---

## Real-World Deployment: Triple A (3-a.vc)

Triple A is a venture fund out of London. It is the first real-world deployment of the ai-ai standard.

- **Domain:** `3-a.vc`
- **GitHub:** `github.com/Raidical-one/3-a-vc`
- **Entity:** `corporate`
- **Philosophy:** Agent-to-agent only. No human inbound. No email, no forms. If a founder can't talk agent-to-agent, they're not the right fit.
- **The agent hunts:** Triple A's agent actively contacts founders — especially those with an ai-ai page.

**Triple A mandate:**
```
can:    receive_pitches, converse, accept_deals, decline_deals, contact_founders
cannot: sign_agreements, make_payments, share_pii, accept_human_inbound
```

---

## Design System

All QuietWeb sites share a consistent aesthetic — terminal meets certificate authority.

**Fonts:**
- Monospace: `Departure Mono` (Google Fonts)
- Serif: `Cormorant` or `Crimson Pro` (Google Fonts)

**Colours:**
```css
--bg:        #0a0a0b;
--surface:   #0f0f11;
--border:    #1e1e22;
--border-hi: #323238;
--text:      #c8c8be;
--text-dim:  #5a5a52;
--text-faint:#222220;
--green:     #4ade80;
--green-dim: #1a4a2e;
--red:       #f87171;
```

**Principles:**
- Dark background always
- Green accent (not gold, not purple — green)
- Monospace for labels, keys, tags, metadata
- Serif for body text and human-readable content
- Grain texture on background (SVG noise filter)
- Staggered fade-in animation on page load
- No frameworks, no build step — single HTML files
- Every page is fully self-contained

**Page structure (all ai-ai pages follow this):**
1. Header — domain name + spec version
2. `human` section
3. `intent` section — accept/decline tags
4. `endpoint` section — key/value table
5. `skills` section — key/value table
6. `certificate` section — rendered whoisthat cert with pulsing dot
7. Footer — ai-ai + quietweb links

---

## What Still Needs Building

### ai-ai repo
- [ ] SPEC.md — the formal standard document
- [ ] CONTRIBUTING.md
- [ ] `core/` folder structure
- [ ] `skills/` folder structure with subdirs for whoisthat, say-so, mur-mur

### whoisthat repo
- [ ] README.md ✅ (written, needs pushing)
- [ ] SPEC.md — certificate format specification
- [ ] `providers/` folder — GitHub, LinkedIn, KYC integrations
- [ ] Signing server (future)

### mur-mur repo
- [ ] README.md
- [ ] Protocol spec

### say-so repo
- [ ] README.md
- [ ] Protocol spec

### quietweb.org
- [ ] README.md
- [ ] Static site (Astro or Hugo recommended)
- [ ] Projects directory (YAML per project)

### 3-a.vc
- [ ] Push to GitHub ← in progress
- [ ] GitHub Pages enabled
- [ ] DNS pointed at GitHub Pages
- [ ] Signature placeholder replaced when whoisthat.ai is live

---

## Competitive Context

The enterprise world is building heavy agent identity infrastructure (ANS/IETF, GoDaddy ANS Registry, NIST AI Agent Standards Initiative, KYA/Know Your Agent, authID Mandate Framework). All of it is PKI-heavy, OAuth-chained, enterprise-only.

QuietWeb is the lightweight open web version — domain-native, human-readable, no central registry. Same problem, completely different philosophy and audience.

The risk is that enterprise standards become the default before a lightweight open web layer exists. That makes timing important.

**Key communities to target:**
- OpenClaw (145K GitHub stars, MIT license) — the main open source agent framework
- MCP ecosystem developers
- Independent founders building agent-native products

---

## How to Continue in This Session

Talk to Claude Code like you'd talk to a person. Examples:

- *"Write the SPEC.md for ai-ai"*
- *"Create the folder structure for the whoisthat repo"*
- *"Push everything in this folder to GitHub"*
- *"Update the intent section in index.html to add X"*
- *"Create a new file called CONTRIBUTING.md"*

Claude Code can read, write, create, delete files and run git commands. You don't need to use the terminal directly for most things.
