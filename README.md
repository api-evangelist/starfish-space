# Starfish Space

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
