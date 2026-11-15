# Blog

Personal blog of Vadim Tiganov – notes on ML engineering: ranking, recommender
systems, search, NLP and LLM/agentic systems.

Built with **Jekyll + [minima](https://github.com/jekyll/minima)** and published
with **GitHub Pages**.

- Site: https://tiganoviv.github.io/blog/
- Stack: Jekyll, minima theme, no build step required locally

## How it works

GitHub Pages builds the site automatically on every push to `main`. The build
uses the Jekyll version and plugins that Pages supports natively, so there is no
CI workflow to maintain and no `Gemfile` to keep in sync.

Config lives in [`_config.yml`](_config.yml):

- `baseurl: /blog` – because this is a project site
- `theme: minima`, plugins `jekyll-feed` and `jekyll-seo-tag`
- `header_pages` controls which pages appear in the top navigation

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
4. Commit and push to `main` – the site rebuilds in about a minute.

Posts are listed on the home page newest first, with excerpts
(`show_excerpts: true`). The RSS feed is generated at `/blog/feed.xml`.

## Adding a page

Same as a post, but with `layout: page` and a `permalink`, and add the file name
to `header_pages` in `_config.yml` so it shows up in the navigation. Example:
[`about.md`](about.md).

## Local preview (optional)

Requires Ruby. From the repository root:

```bash
gem install jekyll bundler minima jekyll-feed jekyll-seo-tag
bundle exec jekyll serve --baseurl ""
```

Then open http://localhost:4000. Note that `--baseurl ""` is needed to preview a
project site locally.

## Customisation

Visual tweaks live in [`assets/main.scss`](assets/main.scss): it imports the
minima theme and overrides a handful of colours. Everything else comes from
minima, which keeps the stylesheet small and easy to reason about.

## Deployment

GitHub Pages, source: `main` branch, root folder. Nothing else to configure –
pushing to `main` is the deployment.
