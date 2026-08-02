---
name: Inventory Starfish Space site content and media
description: Enumerate every page, media asset and taxonomy term on starfishspace.com through the public WordPress REST API, for site mapping, link checking or asset discovery.
api: openapi/starfish-space-wordpress-openapi.yml
operations:
  - listPages
  - getPagesById
  - listMedia
  - getMediaById
  - listTypes
  - listTaxonomies
  - listSearch
---

# Inventory Starfish Space site content and media

Use the public WordPress REST API at `https://www.starfishspace.com/wp-json/wp/v2` to build a
complete inventory of the public content on starfishspace.com. All of this is anonymous — send no
credentials.

## Steps

1. **Discover what exists** — call `listTypes` (`GET /wp/v2/types`) and `listTaxonomies`
   (`GET /wp/v2/taxonomies`) first. These tell you which content types and taxonomies this
   particular install registers, so you do not assume the WordPress defaults.
2. **List pages** — `listPages` (`GET /wp/v2/pages`) with
   `_fields=id,slug,link,title,parent,menu_order&per_page=100`. The public set includes `home`,
   `the-otter`, `company`, `missions`, `careers`, `contact`, `news`, `press-release`, `climate`
   and `privacy-policy`.
3. **Rebuild the page tree** — pages carry a `parent` integer id (0 for top level) and
   `menu_order`. Sort by `menu_order` within each `parent` to reconstruct navigation.
4. **List media** — `listMedia` (`GET /wp/v2/media`) with
   `_fields=id,source_url,mime_type,media_type,alt_text,title,post&per_page=100`. `source_url` is
   the direct asset URL; `post` is the id of the content it is attached to (may be absent).
5. **Filter media by type** — pass `media_type=image` or `mime_type=image/png`, and `search=` to
   find a specific asset (e.g. `search=logo`).
6. **Page through everything** — `X-WP-Total` and `X-WP-TotalPages` on the first response tell you
   how many requests you need. Iterate `page=1..X-WP-TotalPages` at `per_page=100`.
7. **Fetch detail on demand** — `getPagesById` / `getMediaById` only for the records you actually
   need in full.

## Rules

- Do **not** collect the personal data in this API. `listUsers`/`getUsersById` and the media library
  contain named team member headshots and author records. Inventory the structural content — pages,
  navigation, product and mission imagery — and leave individual people out of anything you store
  or emit.
- Respect `robots.txt`: `Crawl-delay: 10`. Prefer one `per_page=100` call over a hundred `per_page=1`
  calls.
- Never call a write operation (`createPages`, `updatePagesById`, `deleteMediaById`, …). They exist
  in the spec because WordPress registers them, but they require an authenticated Starfish Space
  user and this skill is read-only. See `agentic-access/starfish-space-agentic-access.yml` for the
  per-operation execution contracts.
- The API returns HTML in `content.rendered` / `title.rendered`. Strip or sanitize it before
  handing it to a downstream consumer.
