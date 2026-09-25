# Agent Instructions for serral.github.io

This is a Jekyll-based personal GitHub Pages site, served at https://www.serralheiro.uk.

## Hosting & Domains

GitHub Pages is the only host. Deploying = pushing to `master` (GitHub builds Jekyll).

| Name | How it is served |
|------|------------------|
| `www.serralheiro.uk` | Canonical site. GitHub Pages custom domain (`CNAME` file), Cloudflare DNS-only CNAME to `serral.github.io`, domain verified, HTTPS enforced |
| `serral.github.io` | GitHub 301 to `https://www.serralheiro.uk/` |
| `queima.com`, `www.queima.com` | Cloudflare proxied + Redirect Rule, 301 to `https://www.serralheiro.uk` keeping path and query (zone SSL: Full strict) |
| `dove.queima.com` | Server (Contabo), nginx; DNS-only on purpose (SSH, certbot). `/` 301s to the site, nothing else served |
| `serralheiro.uk` (apex) | Intentionally not pointed at GitHub Pages (Pages shows a harmless alt-domain warning) |

- Do not edit or delete `CNAME`; it binds the custom domain
- `url` in `_config.yaml` must stay `https://www.serralheiro.uk` (canonical, og:url, sitemap, robots.txt)
- Never copy the built site to dove or any other server; there is one source of truth

## Build Commands

```bash
# Install dependencies
bundle install

# Build site (production)
bundle exec jekyll build

# Serve locally with live reload
bundle exec jekyll serve --livereload

# Clean build artifacts
bundle exec jekyll clean
```

Note: the macOS system Ruby (2.6) lacks bundler 2.7.1 from `Gemfile.lock`, so these commands need a newer Ruby (e.g. Homebrew or rbenv) to run locally.

## Testing

This is a static Jekyll site with no test framework. Verify changes by:
1. Running `bundle exec jekyll serve` and checking localhost:4000 (or, after pushing, the live site once the Pages build finishes: `gh api repos/serral/serral.github.io/pages/builds/latest`)
2. Validating HTML output with browser dev tools
3. Checking YAML front matter syntax in `_config.yaml`

## Project Structure

```
/
├── _config.yaml      # Jekyll site configuration
├── _includes/        # ascii-art.txt (portrait shown beside content)
├── _layouts/         # HTML layouts (default.html)
├── assets/css/       # Stylesheets (main.css)
├── CNAME             # GitHub Pages custom domain (www.serralheiro.uk)
├── index.html        # Main page with YAML front matter
├── 404.html          # Error page
├── Gemfile           # Ruby dependencies
└── robots.txt        # SEO robots file
```

## Code Style Guidelines

### YAML Front Matter
- Use 2-space indentation in `_config.yaml`
- Front matter at top of files between `---` markers
- Required fields: `layout`, `title`, `description`

```yaml
---
layout: default
title: Page Title
description: Brief description for SEO
sitemap:
  priority: 0.8
  changefreq: monthly
---
```

### HTML/Liquid Templates
- 2-space indentation
- Use Liquid tags for dynamic content: `{{ variable }}`, `{% if %}`
- Prefer `relative_url` filter for internal links
- `<head>` meta tags come from `{% seo title=false %}`; the layout writes `<title>` itself (homepage: site name only, other pages: `Page | Site`)

### CSS
- Use CSS variables in `:root` for colors
- BEM-like naming: `.link-item`, `.container`
- Mobile-first responsive design
- Always include `:focus` styles for accessibility

### File Naming
- Layouts: `default.html`, `post.html`
- Assets: lowercase with hyphens: `main.css`, `site-logo.png`
- Pages: lowercase, descriptive: `index.html`, `about.html`

## Security & SEO

- External links: always use `target="_blank" rel="noopener noreferrer"`
- Include structured data (JSON-LD) for Person schema
- All pages should have unique title and description
- Use the `{% seo %}` tag from jekyll-seo-tag (with `title=false`); do not hand-write description, Open Graph, Twitter or canonical tags, they would duplicate its output
- The homepage `description` in `index.html` must match `description` in `_config.yaml`

## Dependencies

- GitHub Pages gem (frozen at GitHub's versions)
- jekyll-seo-tag for meta tags
- jekyll-sitemap for sitemap generation
- Kramdown for Markdown processing

## Git Workflow

- Commit message: concise, describe the change
- Never commit: `_site/`, `.jekyll-cache/`, secrets, `.env`
- Test locally before pushing to trigger GitHub Pages build
- Pushing to `master` publishes to https://www.serralheiro.uk
