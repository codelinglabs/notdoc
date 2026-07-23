# Notilify API Documentation

This repository contains the public Notilify API reference served at
[docs.notilify.com](https://docs.notilify.com/). The local documentation UI is
rendered by [Scalar](https://github.com/scalar/scalar) from `openapi.json`.

## Run locally

Requirements:

- Python 3
- `jq` for OpenAPI validation
- An internet connection so the browser can load Scalar from the jsDelivr CDN

From the repository root, start a static server:

```bash
python3 -m http.server 8081
```

Then open:

```text
http://127.0.0.1:8081/#description/introduction
```

Stop the server with `Ctrl+C`.

Do not open `index.html` directly from the filesystem. Serving the directory
over HTTP allows Scalar to fetch `openapi.json` correctly.

## Make documentation changes

- Change API endpoints, schemas, authentication, SDK installation guidance, and
  request examples in the backend routes or
  `notilify-be/scripts/openapi-overrides.json`.
- Run `npm run docs:sync:notdoc` from the sibling `notilify-be` repository to
  regenerate and copy `openapi.json` here for review.
- Edit `introduction.mdx` to change the introductory guide.
- Edit `index.html` to change the Scalar setup, page metadata, or favicon.
- Edit `docs.json` and `api-reference/` only when changing the Mintlify
  configuration retained in this repository.

Do not edit `openapi.json` directly in this repository. It is a generated copy
of the backend contract, and direct edits will fail the cross-repository
contract gate.

Refresh the browser after saving. If Scalar still shows an older document,
perform a hard refresh.

## Validate changes

Validate the OpenAPI file as JSON:

```bash
jq empty openapi.json
```

Check the working-tree diff before committing:

```bash
git diff --check
git diff
```

This repository does not currently have an automated test runner or build
command. Local verification consists of validating `openapi.json` and opening
the affected documentation sections in the browser.

Before a public contract or SDK release, run the complete gate from the sibling
SDK repository:

```bash
cd ../notilify-be
./scripts/check-notilify-contract.sh
```

## Repository structure

```text
index.html          Scalar documentation entry point
openapi.json        API contract, schemas, examples, and SDK snippets
introduction.mdx    Introduction content
favicon.svg         Browser favicon
docs.json           Mintlify configuration
api-reference/      Mintlify API-reference content
images/             Documentation images
logo/               Light and dark logo assets
snippets/           Reusable documentation snippets
```
