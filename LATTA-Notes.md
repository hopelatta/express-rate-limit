# NOTES — express-rate-limit (fork)

## Why I forked this
I’m using `express-rate-limit` as a reference point for **platform integrity controls**.  
Rate limits are one of the simplest tools that meaningfully reduce abuse at scale (spam, scraping, credential stuffing, brute force, and high-velocity messaging patterns).

I’m specifically interested in how “generic web security rate limiting” maps onto **relationship-driven platforms**, where limits also shape pacing, safety, and user trust.

## What I’m looking at in this codebase
- **Key generation**: what is used to identify a “client” (IP by default, but often not sufficient)
- **Store behavior**: in-memory vs Redis vs other distributed stores; correctness under load
- **Header standards**: how rate-limit information is exposed to clients (useful, but can be gamed)
- **Skip logic**: safe patterns for allowlists and internal traffic
- **Handler behavior**: what happens when a limit is hit (block vs degrade vs delay)

## Integrity takeaways for dating / messaging systems
In a relationship platform, rate limits are not only about abuse prevention. They also:
- reduce spammy “spray and pray” outreach
- limit harassment bursts
- slow down scripted account creation
- create **progressive friction** for risky behavior without immediate bans

But naive limits can harm legitimate users:
- shared networks (campus, shelters, workplaces)
- mobile carrier NAT
- VPN usage
- users traveling or on unstable IPs

## Practical adaptations I’m thinking about
- Prefer **account- and device-level** limiting where appropriate, not only IP
- Use **tiered limits** by account age / verification level
- Use **endpoint-specific** limits (e.g., first messages, link sends, photo sends) rather than one global bucket
- Consider **cooldowns and delays** as alternatives to hard blocks for borderline cases
- Pair limits with **risk signals** (velocity + reciprocity + prior enforcement), not raw volume alone

## Risks / tradeoffs to keep in mind
- **Fairness**: IP-based controls can disproportionately impact users on shared networks
- **Privacy**: device fingerprinting can be invasive if done carelessly
- **Gaming**: attackers adapt quickly to static thresholds
- **User trust**: silent limiting feels like “the app is broken”

## Questions to explore next
- What is the safest default for `keyGenerator` in modern environments with proxies/CDNs?
- How do different stores behave under burst traffic and multi-instance deployments?
- What are good patterns for “soft limiting” (delay responses) vs “hard limiting” (429)?
- How can limits be made legible to users without teaching attackers?

## Link back to my broader notes
See also my repo: `platform-integrity-systems/rate-limiting.md` for dating-platform-specific framing.
