# Spectrum Suite Docs

Documentation for the Spectrum Suite API, built with [Mintlify](https://mintlify.com).

## Structure

```
docs/
├── api-guide/                 # Conceptual guides
│   ├── importing-and-querying-data.mdx
│   ├── matching-business-entities.mdx
│   ├── matching-line-items-legacy.mdx
│   └── workflow-overview.mdx
├── api-reference/             # Endpoint reference pages
│   ├── openapi.json           # OpenAPI spec (auto-generated from API)
│   └── {entity}/              # One directory per API entity
├── core-concepts/
│   └── business-glossary.mdx
├── getting-started/
│   └── authentication.mdx
├── images/
│   └── logos/                 # Site logo and favicon
├── docs.json                  # Site config and navigation
└── index.mdx                  # Landing page
```

## Local development

```bash
npm i -g mint
mint dev
```

Preview at `http://localhost:3000`.

## Updating the API spec

The `api-reference/openapi.json` file is generated from the running Spectrum Suite API:

```
GET https://spectrumsuite-dev.azurewebsites.net/openapi/v1.json
```

Replace `api-reference/openapi.json` with the downloaded spec, then restart `mint dev`.

## Checking for broken links

```bash
mint broken-links
```

> Note: pages that use `openapi:` frontmatter are reported as broken by `mint broken-links` due to a known limitation of the Mintlify v4 CLI — they resolve correctly in the live site.
