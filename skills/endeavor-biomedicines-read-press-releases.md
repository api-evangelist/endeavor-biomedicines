---
name: Read Endeavor BioMedicines press releases
description: >-
  Retrieve Endeavor BioMedicines corporate news and press releases as structured JSON
  from the company's public WordPress REST API, including paging over the full set and
  resolving the categories and featured media attached to each release.
api: openapi/endeavor-biomedicines-wordpress-rest-openapi.yml
operations:
  - getWpV2Posts
  - getWpV2PostsById
  - getWpV2Categories
  - getWpV2Media
---

# Read Endeavor BioMedicines press releases

Endeavor BioMedicines publishes no developer program. Its corporate site is
nonetheless a readable JSON API: the WordPress REST surface at
`https://endeavorbiomedicines.com/wp-json` answers content reads anonymously.

## Before you start

- Base URL: `https://endeavorbiomedicines.com/wp-json`
- No API key, no sign-up, no OAuth for these operations. Do not send credentials.
- No rate limit is published and none is signalled in response headers. Keep request
  volume modest and space requests out; there is no `Retry-After` to obey.

## Steps

1. **List the releases.** Call `getWpV2Posts` — `GET /wp/v2/posts`. Use `per_page`
   (max 100) and `page`. At the time of profiling there were 22 posts.
2. **Page correctly.** Read `X-WP-Total` and `X-WP-TotalPages` from the response
   headers to size the walk, or follow the `Link: rel="next"` header (RFC 8288).
   Both are CORS-exposed via `Access-Control-Expose-Headers`. Stop when there is no
   `rel="next"`.
3. **Trim the payload.** Add `_fields=id,date,slug,link,title,categories,featured_media`
   so you do not pull the full rendered HTML of every release. The collection is
   ~200KB unfiltered.
4. **Resolve linked objects in one pass.** Add `_embed` to inline the author, featured
   media and terms rather than issuing follow-up calls. Otherwise call
   `getWpV2Categories` and `getWpV2Media` for the ids on each post.
5. **Fetch one release in full.** Call `getWpV2PostsById` — `GET /wp/v2/posts/{id}` —
   for the rendered `content.rendered` body of a specific release.
6. **Filter by date or search.** `after` / `before` (ISO 8601), `modified_after`,
   `search`, `categories`, `slug`, `orderby=date&order=desc`.

## Handling failures

- Errors return `{"code","message","data":{"status"}}` as `application/json` — not
  RFC 9457 problem+json. Branch on `code`, not on the message string.
- `rest_invalid_param` (400) means a parameter failed the registered schema; check the
  enum or default in the OpenAPI.
- `rest_post_invalid_id` (404) means the id does not exist or is not public.
- These are read operations, so a retry is safe. There is **no** idempotency key on
  this API — never assume that safety carries over to a write.

## Do not

- Do not attempt writes. `POST`, `PUT` and `DELETE` on any `/wp/v2` route return
  401/403 without a WordPress session or bearer token, and API Evangelist holds none.
- Do not call the MCP endpoint from this skill; it is a separate, gated surface. See
  `skills/endeavor-biomedicines-inspect-mcp-surface.md`.
