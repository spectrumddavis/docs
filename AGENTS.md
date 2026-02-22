# Documentation project instructions

## About this project

This is the documentation site for **Spectrum Suite**, built on [Mintlify](https://mintlify.com).

- Pages are `.mdx` files with YAML frontmatter
- Navigation is configured in `docs.json`
- Run `mint dev` to preview locally
- Run `mint broken-links` to check for broken links

The docs cover two main areas:

- **Guides** (`api-guide/`, `getting-started/`) — conceptual, high-level content explaining what things are and when to use them
- **API Reference** (`api-reference/`) — technical specification: endpoints, field tables, enumerations, response shapes

---

## Terminology

Use these exact names and casings at all times:

| Term | Notes |
|------|-------|
| Spectrum Suite | The product — always two words, title case |
| ABBYY Vantage | Document capture platform |
| ABBYY FlexiCapture | Legacy document capture platform |
| Therefore™ | Document management platform |
| Toca® | Low-code automation platform |

---

## Localisation

Write in **British English (en-GB)**:

- "authorisation" not "authorization"
- "organisation" not "organization"
- "recognise" not "recognize"

**Exception:** Keep technical identifiers exactly as they appear in code — e.g. `sourceSystem`, `Authorization` header name, JSON field names.

---

## Style

- Use active voice and second person ("you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings at every level (H1–H4) — not title case. Only proper nouns, product names, and acronyms keep their capitalisation. See the [Google Developer Documentation Style Guide](https://developers.google.com/style/capitalization#capitalization-in-titles-and-headings) for the canonical reference.
- **Bold** for UI elements: click **Save**, open **Settings**
- `Code formatting` for file names, commands, paths, API field names, and inline code
- Avoid filler phrases: "simply", "just", "easy", "straightforward"

---

## Content boundaries

| Section | What belongs here |
|---------|-------------------|
| Guides | What it is, when to use it, how it fits the workflow — no field tables or enumerations |
| API Reference | Field definitions, request/response shapes, enumerations, status codes, query parameters |

Do not document internal admin features.

---

## Navigation and linking

- All internal links use root-relative paths with **no `.mdx` extension**: `/api-reference/suppliers/query`
- Link text should match the destination page title exactly
- Entity groups in `docs.json` are ordered **alphabetically**
- When adding a new entity, create a subdirectory under `api-reference/` (e.g. `api-reference/widgets/`) and add it to the correct alphabetical position in `docs.json`

### Navigation title case

Navigation labels in `docs.json` use **title case** — this is distinct from the sentence-case rule that applies to MDX page headings. The tab names and group names in `docs.json` are UI navigation elements, not document headings.

- Tab names: `"Guides"`, `"API Reference"`
- Group names: `"Getting Started"`, `"Core Concepts"`, `"API Guide"`, `"Common Patterns"`, `"Addresses"`, etc.

Single-word group names are unambiguous; apply title case to every word in multi-word names.

---

## API conventions

### Request headers

All tenant-scoped endpoints require:

```
Authorization: Bearer {token}
X-Tenant-Id: {tenantId}
```

The legacy matching endpoint (`/api/legacy/matching/invoice-line-item`) does **not** require `X-Tenant-Id`.

### Response envelope

All standard endpoints return:

```json
{
  "data": {},
  "message": null
}
```

The legacy matching endpoint returns a flat response object — no envelope.

### Pagination

Query endpoints use cursor-based pagination via `PaginationOptions`:

| Parameter | Type | Default |
|-----------|------|---------|
| `cursor` | `int?` | — |
| `pageSize` | `int` | 100 |

Paged responses include: `results`, `nextCursor` (int?), `hasNextPage` (bool), `returned` (int).

### Upsert identity

Records are matched (insert or update) on the combination of `sourceSystem` + `sourceSystemId`.

### Enumerations

Document enum fields using their **string representations**, not integer values — even when the underlying API type is an integer enum.

- Single value: `"Multiply"` or `"Divide"`
- Flags enum (multiple values): comma-separated string — `"QuantityAndUnitPrice, ArticleNumberAndQuantityAndUnitPrice"`

In field tables, use `string` as the type and list the accepted values in the description, e.g.: `Conversion direction: \`"Multiply"\` or \`"Divide"\``. For flags enums, reference the full name table and show a combining example in the description.

### Match confidence scores

Match endpoints return `confidenceScore` as a decimal between `0.0` and `1.0`, where `1.0` = 100% confidence. Standard thresholds:

- **0.70–1.0** — very likely match; safe to auto-action without review
- **0.50–0.70** — likely match; recommend manual review before actioning
- **0.30–0.50** — possible match; review against source data
- **0.00–0.30** — weak match; discard or investigate data quality

---

## Common patterns

Three shared pattern pages document the canonical behaviour for each endpoint type. Entity-specific pages reference these rather than repeating the detail.

| Pattern | Page | Applies to |
|---------|------|------------|
| Paginated query | `/api-reference/common-paginated-query` | `GET /api/{entity}` |
| Get single | `/api-reference/common-get-single-record` | `GET /api/{entity}/{id}` |
| Batch upsert | `/api-reference/common-batch-upsert` | `PUT /api/{entity}/batch-upsert` |

---

## Not-implemented endpoints

`POST /api/invoice-line-items/match` returns HTTP 501. Direct users to the legacy matching endpoint instead: `POST /api/legacy/matching/invoice-line-item`.

---

## Mintlify components and features

Prefer Mintlify components over plain Markdown equivalents wherever they improve clarity. The following are in active use or worth applying when adding or editing content.

### In active use

#### `<Steps>` / `<Step>`
Use for sequential procedures where order matters — installation, configuration flows, multi-stage API calls.
```mdx
<Steps>
  <Step title="Step title">
    Step body. Can include code blocks, lists, callouts.
  </Step>
</Steps>
```
Do **not** use for unordered investigation checklists or conceptual bullet lists — plain `1. 2. 3.` or `-` is fine there.

#### `<CardGroup>` / `<Card>`
Use for "next steps" or navigation sections. Replaces plain bullet link lists with visual, clickable cards.
```mdx
<CardGroup cols={2}>
  <Card title="Card title" icon="icon-name" href="/path">
    Short description.
  </Card>
</CardGroup>
```
Icon names come from [Lucide](https://lucide.dev) or Font Awesome.

#### `<CodeGroup>`
Use when two or more code blocks are alternatives or a natural pair (request + response, same call in multiple platforms/languages). Renders as tabbed blocks.
```mdx
<CodeGroup>
```bash curl
curl ...
```

```json Response
{ ... }
```
</CodeGroup>
```
Do **not** use across section breaks or for sequential blocks that aren't alternatives.

#### Named code blocks
Add a display label after the language identifier. Use on any block where the label adds context (e.g. when multiple blocks appear on a page).
````mdx
```json Response
{ ... }
```
````
Common labels: `Request`, `Response`, `200 OK`, `400 Bad Request`, `404 Not found`, `JavaScript`, `curl`, platform names.

#### Reusable snippets
Create `.mdx` files in `snippets/`, import with `import X from '/snippets/x.mdx'`, then use as `<X />`.
Currently in use:
- `snippets/required-headers.mdx` — `Authorization` + `X-Tenant-Id` table (GET endpoints)
- `snippets/required-headers-with-content-type.mdx` — above + `Content-Type` (PUT/POST endpoints)

Add new snippets when the same content block appears on more than one page.

#### Callouts
Choose the severity that matches the content:
- `<Note>` — supplementary info, safe to skip
- `<Info>` — helpful context (permissions, environment differences)
- `<Tip>` — recommendations and best practices
- `<Warning>` — potentially destructive or easy-to-miss gotchas
- `<Check>` — success confirmation

#### API playground
Entity pages include `openapi:` frontmatter pointing to `api-reference/openapi.json`. The `api.baseUrl` and `api.auth.method: "bearer"` are configured in `docs.json`. The playground is live — authenticated users can make real API calls directly from the docs.