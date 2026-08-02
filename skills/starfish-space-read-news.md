---
name: Read Starfish Space news and press releases
description: Retrieve, filter and page through the news and press releases Starfish Space publishes, using its public WordPress REST API — no credentials required.
api: openapi/starfish-space-wordpress-openapi.yml
operations:
  - listPosts
  - getPostsById
  - listCategories
  - listTags
---

# Read Starfish Space news and press releases

Starfish Space publishes its company news and press releases through the public WordPress REST API
at `https://www.starfishspace.com/wp-json/wp/v2`. Read access needs **no credential** — do not send
an Authorization header.

## Steps

1. **List posts** — call `listPosts` (`GET /wp/v2/posts`). Page with `page` and `per_page`
   (`per_page` max is 100, default 10). Sort with `orderby` + `order`.
2. **Narrow the payload** — pass `_fields=id,date,slug,link,title,excerpt` so you do not pull the
   full rendered HTML of every post. Pass `_embed` only when you actually need the author, terms or
   featured image inlined.
3. **Filter by date** — `after` / `before` (and `modified_after` / `modified_before`) take ISO 8601
   datetimes. Use these for "what changed since" rather than fetching everything.
4. **Search** — `search=<string>` on `listPosts`, or `listSearch` (`GET /wp/v2/search`) to search
   across both posts and pages at once.
5. **Follow pagination** — read `X-WP-Total` and `X-WP-TotalPages` from the response headers, or
   follow `rel="next"` in the RFC 8288 `Link` header. Stop when `page` exceeds `X-WP-TotalPages`;
   asking beyond the last page returns a 400.
6. **Fetch one post** — `getPostsById` (`GET /wp/v2/posts/{id}`) for the full record. The rendered
   body is `content.rendered`; the title is `title.rendered`.
7. **Resolve taxonomies** — post objects carry `categories[]` and `tags[]` as integer ids. Resolve
   them with `listCategories` / `listTags` using `include=<comma-separated ids>` in one call, rather
   than one request per term.

## Rules

- The `/wp/v2/posts` collection can legitimately return `[]` — Starfish Space keeps press releases
  under a separate route structure and the default posts collection may be empty. Treat an empty
  array as a valid answer, not an error, and fall back to `listPages` or `listSearch`.
- Errors are **not** RFC 9457. The envelope is
  `{"code": "...", "message": "...", "data": {"status": <int>}}` — branch on `code`, not on the
  message text. See `errors/starfish-space-problem-types.yml`.
- There is **no idempotency contract** on this API, and no published rate limit. Be conservative:
  the host runs a cache plugin and Wordfence, either of which may throttle. `robots.txt` asks for a
  10-second crawl delay — honor it for bulk reads.
- This API serves website content only. It exposes nothing about spacecraft, orbits, telemetry or
  missions beyond what is written in the published pages.
