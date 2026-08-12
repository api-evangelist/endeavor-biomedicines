---
name: Inspect the Endeavor BioMedicines MCP surface
description: >-
  Determine what the WordPress MCP server on endeavorbiomedicines.com exposes and what
  it requires, by reading the anonymous OAuth 2.1 discovery documents and confirming
  the endpoint's authentication gate — without attempting to obtain a token.
api: openapi/endeavor-biomedicines-wordpress-rest-openapi.yml
operations:
  - getMcp
  - getWpabilitiesV1
  - getWpabilitiesV1Abilities
---

# Inspect the Endeavor BioMedicines MCP surface

The host runs a WordPress MCP adapter. The endpoint is real and live, but gated.
This skill is about establishing that fact accurately — not about getting in.

## Steps

1. **Read the protected-resource metadata.**
   `GET https://endeavorbiomedicines.com/.well-known/oauth-protected-resource`
   (RFC 9728). It names the resource
   `https://endeavorbiomedicines.com/wp-json/mcp/mcp-oauth-server`, its authorization
   server, `bearer_methods_supported: ["header"]` and `scopes_supported: ["mcp"]`.
2. **Read the authorization-server metadata.**
   `GET https://endeavorbiomedicines.com/.well-known/oauth-authorization-server`
   (RFC 8414): authorization, token and revocation endpoints;
   `grant_types_supported` = `authorization_code`, `refresh_token`;
   `code_challenge_methods_supported` = `S256`;
   `token_endpoint_auth_methods_supported` = `none` (public clients);
   `client_id_metadata_document_supported` = `true` — there is no registration
   endpoint, so a client identifies itself with a client-ID metadata document.
3. **Confirm the gate, once.** POST
   `{"jsonrpc":"2.0","id":1,"method":"tools/list"}` to the MCP endpoint with
   `Accept: application/json, text/event-stream`. Expect
   `401 {"code":"mcp_unauthorized","message":"MCP authentication required."}`.
   Record the status. Do not retry in a loop and do not probe for weaknesses.
4. **Look at the likely backing surface.** Call `getWpabilitiesV1` and
   `getWpabilitiesV1Abilities` — `GET /wp-json/wp-abilities/v1[/abilities]` — the
   WordPress Abilities API the adapter projects tools from. Treat any binding between
   an ability and an MCP tool as unverified until `tools/list` answers.

## Conclusion to record

The tool set is unknown and must stay unknown. Write down "gated", not a guessed list.
`mcp/endeavor-biomedicines-tool-crosswalk.yml` deliberately holds zero mapped rows for
this reason.

## Do not

- Do not attempt to obtain an OAuth token, register a client, or authenticate.
- Do not infer tool names from other WordPress MCP installs and attribute them to this
  provider.
