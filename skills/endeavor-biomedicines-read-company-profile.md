---
name: Read the Endeavor BioMedicines company profile
description: >-
  Pull the company's pipeline, science and therapeutic-area pages, plus its leadership,
  board and advisor roster, as structured JSON from the public WordPress REST API,
  using the type and taxonomy registries to discover what is available first.
api: openapi/endeavor-biomedicines-wordpress-rest-openapi.yml
operations:
  - getWpV2Types
  - getWpV2Taxonomies
  - getWpV2Pages
  - getWpV2PagesById
  - getWpV2Member
  - getWpV2MemberById
  - getWpV2MemberCat
---

# Read the Endeavor BioMedicines company profile

## Before you start

- Base URL: `https://endeavorbiomedicines.com/wp-json`
- Anonymous. No credentials.
- The company is clinical-stage biotech; nothing here is clinical, regulatory or
  patient data. It is website content.

## Steps

1. **Discover what exists.** Call `getWpV2Types` — `GET /wp/v2/types` — to enumerate
   the registered content types and their `rest_base` values. This install registers
   the standard `post`, `page`, `attachment` types plus a custom `member` type. Call
   `getWpV2Taxonomies` for the taxonomies (`category`, `post_tag`, `member_cat`).
   Do not hardcode the type list; read it.
2. **List the corporate pages.** Call `getWpV2Pages` — `GET /wp/v2/pages` — with
   `_fields=id,slug,link,title,parent` and `per_page=100`. Pages are hierarchical;
   `parent` gives the tree (for example the `env-101`, `pipeline`, `clinical-trials`
   and `publications` pages sit under `our-science`).
3. **Fetch a specific page.** Call `getWpV2PagesById` for the rendered body — for
   example the pipeline page or the fibrotic lung disease therapeutic-area page.
4. **Read the roster.** Call `getWpV2Member` — `GET /wp/v2/member` — for the
   leadership, board and advisor entries, and `getWpV2MemberCat` for the categories
   that separate them. Use `getWpV2MemberById` for one person's entry.
5. **Page and trim** exactly as in the press-release skill: `per_page`, `page`,
   `X-WP-Total`, `X-WP-TotalPages`, `Link: rel="next"`, `_fields`, `_embed`.

## Handling failures

- Error envelope is `{"code","message","data":{"status"}}`.
- A `404 rest_no_route` means the namespace or path is wrong — re-read the live route
  index at `GET /wp-json/` rather than guessing.

## Do not

- Do not scrape the HTML pages when the JSON exists; `content.rendered` on the page
  object is the same content with less breakage.
- Do not treat the roster as personal data to redistribute beyond what the company
  already publishes on its own About pages.
