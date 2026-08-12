# Endeavor BioMedicines

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Endeavor BioMedicines is a clinical-stage biotechnology company headquartered in San Diego,
California, developing medicines aimed at reversing — not merely slowing — the course of
life-threatening fibrotic and oncologic disease. Its lead investigational candidate,
taladegib (ENV-101), is a Hedgehog pathway inhibitor in clinical development for idiopathic
pulmonary fibrosis; it has also in-licensed HMBD-501, a next-generation HER3-targeted
antibody-drug conjugate.

## What was found

**Endeavor BioMedicines runs no developer program.** There is no developer portal, no API
documentation, no pricing, no sign-up, no SDKs, no status page and no published OpenAPI.

There is, however, a real machine-readable surface on the corporate host, and it is profiled
here honestly as what it is — a site-platform API rather than a product API:

- **WordPress REST API** at `https://endeavorbiomedicines.com/wp-json` — 316 routes across 16
  namespaces. Content reads (posts, pages, media, taxonomies, the leadership roster) answer
  anonymously. `openapi/` holds an OpenAPI 3.1 description **derived** by API Evangelist from
  the route index the host itself publishes; the verbatim route index is kept in
  `openapi/_original/`.
- **MCP server** at `/wp-json/mcp/mcp-oauth-server` — live but authentication-gated. An
  anonymous `tools/list` returns `401 mcp_unauthorized`, so **no tool list is recorded**; the
  crosswalk deliberately holds zero mapped rows rather than guessed ones.
- **OAuth 2.1 discovery documents** served anonymously at
  `/.well-known/oauth-authorization-server` (RFC 8414) and `/.well-known/oauth-protected-resource`
  (RFC 9728) — authorization code + refresh token, PKCE S256, single scope `mcp`. Both are
  saved verbatim in `well-known/`.

No A2A agent card, no security.txt, no OpenID configuration, no api-catalog, no ai-plugin —
all probed, all 404, all recorded as absences in `well-known/`.

- Company: https://endeavorbiomedicines.com/
- Secondary market listing: https://forgeglobal.com/endeavor-biomedicines_stock/
