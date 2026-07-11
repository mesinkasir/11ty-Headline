# AGENTS.md

## Scope

This repo is the Ghost **Headline** theme converted to an **Eleventy v4 +
Nunjucks** static site. There is no Ghost backend — posts, pages, authors and
site config are Markdown/YAML under `content/` and `_data/`.

## Commands

```bash
npm install
npm start        # dev server + watch (http://localhost:8080)
npm run build    # production build to _site/ (Pagefind runs on eleventy.after)
```

Node ≥ 22. Eleventy is pinned to `4.0.0-alpha.10` (matches `@awesome.me/buildawesome`).

## Conventions

- Templating is **Nunjucks** (`@11ty/nunjucks` 4). The Eleventy `input` dir is
  `src/` (includes `src/_includes/`, data `src/_data/`, assets `src/assets/`).
  Build tooling — `eleventy.config.js`, `_config/`, `locales/` — stays at the repo
  root; `.js` config files must not live inside `src/` or Eleventy renders them.
- **Drafts**: `published: false` front matter, or a file in `src/_drafts/`
  (directory data defaults `published` to false). A build-mode preprocessor drops
  them from production; they render in `--serve`.
- **Timezone**: `src/_data/metadata.yaml → timezone` (IANA) feeds the date filters
  in `_config/filters.js`, which treat front-matter dates as wall-clock in that
  zone (`keepLocalTime`) so date-only values never drift across UTC.
- Layout chain: content → `_includes/layouts/{post,page,home}.njk` →
  `_includes/layouts/default.njk` (renders `<head>`, header, footer, search modal).
- Shared partials are included with `{% include "partials/NAME.njk" %}` and
  **inherit context**; card/meta partials expect a variable named `post` (an
  Eleventy collection item). Set it before including in a non-loop context.
- Custom filters live in `_config/filters.js`: `t` (i18n echo via
  `locales/en.json`), `readableDate`, `htmlDateString`, `readingTime`, `excerpt`,
  `slug` (re-added — removed in v4), `heroPreload` (LCP), `resolveAuthors`,
  `byTag`, `relatedPosts`, `publicTags`, `year`. Images are optimized by the
  `eleventyImageTransformPlugin` (over `<img>` in built HTML), not a filter.
- Collections (in `eleventy.config.js`): `posts` (newest-first) and `topics`
  (`[{name, count}]` public tags by count).

## Gotchas (Eleventy v4 alpha)

- The `slug` filter was **removed** in v4 → re-added in `_config/filters.js`.
  `slugify` remains built-in.
- Nunjucks namespace member-assignment in `{% set ns.x = … %}` can fail to
  compile — prefer a JS filter (see `relatedPosts`).
- `eleventyComputed` still works despite the internal "buildawesome" rename.

## Features to keep working

- **Pagefind** search: `head.njk` loads the Component UI; `data-pagefind-body`
  on post/page `<article>` scopes the index.
- **Light/dark**: no-flash script in `head.njk`, `assets/css/darkmode.css`,
  `assets/js/theme-toggle.js`, default in `_data/theme.yaml`.
- **Pages CMS**: `.pages.yml` field paths must track `_data/*` and `content/*`.

## Boundaries

- Do not commit `node_modules/`, `_site/`, or secrets.
- Assets under `assets/built/` are the original theme's compiled CSS/JS — treat as
  vendored; add new styles in `assets/css/darkmode.css` or a new file.
