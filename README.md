# Blog

Personal blog of Vadim Tiganov – notes on ML engineering: ranking, recommender
systems, search, NLP and LLM/agentic systems.

Built with **Jekyll + [minima](https://github.com/jekyll/minima)**, rendered by
**GitHub Actions** and served by **GitHub Pages**.

- Site: https://tiganoviv.github.io/blog/
- Stack: Jekyll 4, minima theme, a few Jekyll plugins, no Node, no external CMS

## How it works

Every push to `main` triggers [`.github/workflows/pages.yml`](.github/workflows/pages.yml):

1. installs Ruby and the gems from [`Gemfile`](Gemfile);
2. runs `bundle exec jekyll build` (output goes to `_site/`);
3. uploads `_site` as a Pages artifact and deploys it.

The site is **not** built by the legacy GitHub Pages builder, because that one
only allows a whitelisted set of plugins. Building in Actions means any plugin
can be used – the same approach as larger Jekyll blogs (e.g.
[yegor256/blog](https://github.com/yegor256/blog), which deploys a prebuilt
`_site` to a `gh-pages` branch).

Plugins in use ([`_config.yml`](_config.yml)):

| Plugin | What it does |
|---|---|
| `jekyll-feed` | RSS at `/blog/feed.xml` |
| `jekyll-seo-tag` | meta tags, Open Graph, JSON-LD |
| `jekyll-sitemap` | `/blog/sitemap.xml` |
| `jekyll-paginate-v2` | home page pagination, 5 posts per page |
| `jekyll-archives` | one page per tag at `/blog/tags/<tag>/` |

## Repository layout

```
_config.yml          site config: theme, contacts, plugins, pagination, tags
Gemfile              gem list (pinned with ~>)
index.html           home page, paginated via jekyll-paginate-v2
tags.md              /tags/ – list of all tags with post counts
about.md             /about/ – short bio and links
_layouts/            home.html (paginated list), tag-archive.html
_includes/header.html  name on the left, contact icons on the right
_includes/contact-icon.html  inline SVG glyphs for the contact links
assets/main.scss     minima + colour and layout overrides
_posts/              the posts themselves
.github/workflows/   build and deploy pipeline
```

## Header contacts

Contacts live in `_config.yml` as a list. Each entry has `label` (used for the
accessible name and the tooltip, not shown) and `icon`, which selects a glyph in
[`_includes/contact-icon.html`](_includes/contact-icon.html). Adding a network
means adding a `when` branch there plus an entry in the config.

## Adding a post

1. Create a file in `_posts/` named `YYYY-MM-DD-some-slug.md`.
2. Start it with front matter:

   ```yaml
   ---
   layout: post
   title: "Your title"
   date: 2026-11-15
   tags: [ml, ranking]
   ---
   ```

3. Write Markdown below the front matter.
4. Commit and push to `main`.

Posts appear on the home page newest first, with excerpts
(`show_excerpts: true` in `_config.yml`), and 5 per page.

## Tags

Every tag in a post's front matter automatically gets a page at
`/blog/tags/<tag>/` – no file to create. The `/tags/` page lists all tags.

Keep tags **single words and lowercase** (`ml`, `ranking`, `airflow`): the tag
page URL is built from the tag name lowercased, so multi-word tags produce URLs
with spaces.

## Adding a page

Same as a post, but with `layout: page` and a `permalink`, plus the file name in
`header_pages` in `_config.yml` so it shows up in the navigation. Examples:
[`about.md`](about.md), [`tags.md`](tags.md).

## Local preview (optional)

Requires Ruby 3+. From the repository root:

```bash
bundle install
bundle exec jekyll serve --baseurl ""
```

Then open http://localhost:4000. The `--baseurl ""` flag is needed because the
site normally lives under `/blog`; without it links in the preview point to
`/blog/...`.

## Customisation

Visual tweaks live in [`assets/main.scss`](assets/main.scss): it imports the
minima theme and overrides colours, the header layout, pagination and tags.
Everything else comes from minima, which keeps the stylesheet small and easy to
reason about.

## Deployment

GitHub Pages with **Source: GitHub Actions**
(`https://github.com/tiganoviv/blog/settings/pages`). Pushing to `main` is the
deployment; the workflow takes about a minute. Build logs and history:
`https://github.com/tiganoviv/blog/actions`.
