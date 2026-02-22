# Contributing to Spectrum Suite Docs

## Local development

1. Install the Mintlify CLI: `npm i -g mint`
2. Clone this repository
3. Create a branch: `git checkout -b your-branch-name`
4. Run `mint dev` and preview at `http://localhost:3000`
5. Commit and open a pull request

## Writing guidelines

See [AGENTS.md](AGENTS.md) for the full style guide, terminology, and content conventions.

Key rules:
- British English (en-GB): "authorise", "organise", "recognise"
- Sentence case for all headings — not title case
- Active voice and second person ("you")
- **Bold** for UI elements, `code` for field names, commands, and paths
- No filler words: "simply", "just", "easy"

## Content boundaries

| Section | What belongs here |
|---|---|
| `api-guide/` | Conceptual content — what it is, when to use it |
| `api-reference/` | Field definitions, request/response shapes, enumerations |

## Updating the OpenAPI spec

Fetch a fresh spec from the dev API and replace `api-reference/openapi.json`. See `README.md` for the endpoint URL.
- **Keep sentences concise**: Aim for one idea per sentence
- **Lead with the goal**: Start instructions with what the user wants to accomplish
- **Use consistent terminology**: Don't alternate between synonyms for the same concept
- **Include examples**: Show, don't just tell
