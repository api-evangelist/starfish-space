# Starfish Space

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Starfish Space is a satellite-servicing company founded in 2019 by Trevor Bennett and Austin Link,
headquartered in the Seattle, Washington area (Tukwila). It builds **Otter**, a small, low-cost,
electrically propelled servicing vehicle that performs autonomous rendezvous, proximity operations
and docking (RPOD) with satellites that were never designed to be serviced — extending their life,
relocating them, or disposing of them at end of mission.

- Website: https://www.starfishspace.com/
- The Otter: https://www.starfishspace.com/the-otter/
- Missions: https://www.starfishspace.com/missions/
- News: https://www.starfishspace.com/news/

## API surface

Starfish Space publishes **no developer product API**, no developer portal and no API
documentation. There is no public spacecraft, telemetry, rendezvous or mission-data API. Two real
machine-readable surfaces are served from `www.starfishspace.com` and are catalogued here:

| Surface | Endpoint | Auth |
|---|---|---|
| WordPress REST API (site content) | `https://www.starfishspace.com/wp-json/wp/v2` | anonymous read; WordPress user for writes |
| Model Context Protocol server | `https://www.starfishspace.com/wp-json/mcp/mcp-oauth-server` | OAuth 2.1 + PKCE (S256), scope `mcp` |

The OpenAPI in `openapi/` was **derived** by the API Evangelist enrichment pipeline from documents
the API itself publishes — the route index at `/wp-json/wp/v2` and the item JSON Schemas returned by
HTTP `OPTIONS` on each collection route (both saved verbatim). No paths, parameters or schema
properties were invented. The MCP tool inventory is auth-gated: anonymous `tools/list` returns
`401 mcp_unauthorized`, so no tool list is recorded.

Not found (probed 2026-08-02): `/llms.txt`, `/openapi.json`, `/.well-known/security.txt`,
`/.well-known/api-catalog`, `/.well-known/openid-configuration`, `/.well-known/agent-card.json`,
`/.well-known/agent.json`, status page, changelog, SDKs, CLI, Postman collection, GitHub
organization.

Part of the [API Evangelist](https://apievangelist.com) network. Indexed at
[APIs.io](https://apis.io).
