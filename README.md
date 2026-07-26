# The Empowerment Forge

The official website of The Empowerment Forge, LLC.

## Architecture

This is a dependency-free static website built with plain HTML and embedded
CSS. It has no framework, package manager, JavaScript application, or build
step.

Key files:

- `index.html` — homepage, metadata, and structured data
- `privacy.html` — privacy policy
- `404.html` — custom not-found page
- `robots.txt` — crawler directives
- `sitemap.xml` — indexable page list
- `_headers` — security headers applied by Cloudflare Pages
- `images/` — existing brand assets

## Deployment

The production site is hosted by Cloudflare Pages and connected to the
`empowerment-forge/empowerment-forge.com` GitHub repository.

```text
Cloudflare
    ↓
Cloudflare Pages
    ↓
GitHub (main)
```

The Cloudflare Pages project should use:

- Production branch: `main`
- Framework preset: None
- Build command: `exit 0`
- Build output directory: `.`

Pushing an approved commit to `main` triggers a production deployment.
Pull requests may be used for Cloudflare Pages preview deployments before
merging.

## Canonical Domain and DNS

The canonical website URL is:

```text
https://empowerment-forge.com/
```

DNS and Cloudflare should be configured so that:

- `empowerment-forge.com` is the custom domain for the Pages project.
- HTTP requests redirect to HTTPS.
- `www.empowerment-forge.com` resolves through Cloudflare and redirects with
  HTTP 301 to `https://empowerment-forge.com`, preserving paths and query
  strings.
- The Pages-provided `*.pages.dev` production URL redirects to the canonical
  domain if it is publicly accessible.

Hostname redirects must be configured in Cloudflare Redirect Rules or Bulk
Redirects. Cloudflare Pages `_redirects` files do not support domain-level
redirects.

## Local Preview

From the repository root, run:

```sh
python3 -m http.server 8000
```

Then open `http://localhost:8000/`.

The local Python server may not reproduce Cloudflare Pages extensionless HTML
routes or custom 404 behavior exactly. Production behavior should be verified
on a Cloudflare preview deployment.

## Production Workflow

1. Create a branch and make the required static-file changes.
2. Preview locally.
3. Open a pull request and review the Cloudflare Pages preview deployment.
4. Validate metadata, links, accessibility, crawl files, and response codes.
5. Merge the approved pull request into `main`.
6. Confirm the Cloudflare Pages production deployment succeeds.
7. Verify the canonical domain, redirects, `robots.txt`, `sitemap.xml`, and
   unknown-route 404 response in production.

Do not commit secrets, Cloudflare credentials, or webmaster-tool verification
tokens unless a verification method explicitly requires a public HTML file or
meta tag.
