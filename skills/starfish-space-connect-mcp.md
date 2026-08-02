---
name: Connect to the Starfish Space MCP server
description: Discover and authenticate against the OAuth 2.1 protected Model Context Protocol server Starfish Space serves from its own host.
api: mcp/starfish-space-mcp.yml
operations:
  - initialize
  - tools/list
---

# Connect to the Starfish Space MCP server

Starfish Space serves a live MCP endpoint from `https://www.starfishspace.com/wp-json/mcp/mcp-oauth-server`.
It is **OAuth 2.1 protected** — anonymous `initialize` and `tools/list` calls return HTTP 401
`mcp_unauthorized`. The tool inventory is therefore not public, and nothing in this repository
claims to know what tools it exposes.

## Steps

1. **Discover the protected resource** — `GET https://www.starfishspace.com/.well-known/oauth-protected-resource`
   (RFC 9728). It returns the resource identifier
   (`https://www.starfishspace.com/wp-json/mcp/mcp-oauth-server`), the authorization server, the
   bearer method (`header`) and the supported scopes (`mcp`).
2. **Discover the authorization server** — `GET https://www.starfishspace.com/.well-known/oauth-authorization-server`
   (RFC 8414). Issuer is `https://www.starfishspace.com`; endpoints are `/oauth/authorize`,
   `/oauth/token` and `/oauth/revoke`.
3. **Register or identify the client** — the server advertises
   `token_endpoint_auth_methods_supported: ["none"]` and `client_id_metadata_document_supported: true`,
   so a public client authenticates with a client-id metadata document rather than a client secret.
4. **Run the authorization code flow with PKCE** — `code_challenge_methods_supported` is `["S256"]`
   only; a plain challenge will be rejected. Request the `mcp` scope. Grants supported are
   `authorization_code` and `refresh_token`.
5. **Call MCP** — POST JSON-RPC to the server URL with
   `Authorization: Bearer <token>`, `Content-Type: application/json` and
   `Accept: application/json, text/event-stream`. Send `initialize` first, then `tools/list`.
6. **Revoke when done** — POST the token to `/oauth/revoke`.

## Rules

- Do not assume any tool exists. Enumerate with `tools/list` after authenticating and use the
  `inputSchema` the server returns; this repository records the tool list as `status: gated`
  precisely because it could not be observed.
- A 401 with `{"code":"mcp_unauthorized"}` means the token is missing, expired or lacks the `mcp`
  scope — not that the endpoint is gone.
- There is a second endpoint, `/wp-json/mcp/mcp-adapter-default-server`, which is guarded by
  ordinary WordPress REST authentication and returns `rest_forbidden` anonymously. Prefer the
  OAuth-protected server.
- Access is not self-serve: Starfish Space publishes no sign-up, developer portal or credential
  issuance flow. Contact them at https://www.starfishspace.com/contact/ before attempting to
  register a client.
