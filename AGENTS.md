# Agent Instructions for serral.github.io

This is a Jekyll-based personal GitHub Pages site.

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

## Testing

This is a static Jekyll site with no test framework. Verify changes by:
1. Running `bundle exec jekyll serve` and checking localhost:4000
2. Validating HTML output with browser dev tools
3. Checking YAML front matter syntax in `_config.yaml`

## Project Structure

```
/
├── _config.yaml      # Jekyll site configuration
├── _layouts/         # HTML layouts (default.html)
├── assets/css/       # Stylesheets (main.css)
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
- Include SEO meta tags in `<head>`

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
- Use `{% seo %}` tag from jekyll-seo-tag plugin

## Dependencies

- GitHub Pages gem (frozen at GitHub's versions)
- jekyll-seo-tag for meta tags
- jekyll-sitemap for sitemap generation
- Kramdown for Markdown processing

## Git Workflow

- Commit message: concise, describe the change
- Never commit: `_site/`, `.jekyll-cache/`, secrets, `.env`
- Test locally before pushing to trigger GitHub Pages build
