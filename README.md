# 11ty-Headline

**Headline**, the [Ghost](https://ghost.org) local-news theme, rebuilt as an
[Eleventy](https://www.11ty.dev/) site. Handlebars → Nunjucks, with static
[Pagefind](https://pagefind.app/) search, a light/dark mode switch, and
[Pages CMS](https://pagescms.org/) for browser-based editing.

Original theme © Ghost Foundation (MIT). 

Eleventy conversion by Adam DJ Brett.

## Stack

- **[@11ty/eleventy](https://www.11ty.dev/) `4.0.0-alpha.10`** (the "Build
  Awesome" v4 line) with the bundled **`@11ty/nunjucks` 4** templating engine.
- **[@11ty/eleventy-img](https://www.11ty.dev/docs/plugins/image/)** — the
  `eleventyImageTransformPlugin` optimizes every `<img>` in the built HTML to
  AVIF/WebP with a responsive `srcset` and explicit dimensions.
- **[@awesome.me/buildawesome](https://www.npmjs.com/package/@awesome.me/buildawesome)**
  (installed; pins the Eleventy v4 alpha).
- **[Pagefind](https://pagefind.app/)** — indexes `_site` after every build and
  provides the modal **Component UI** (`<pagefind-modal>`).
- `@apleasantview/eleventy-plugin-baseline` is **optional** — it is not
  installed by default because its peer range rejects the v4 alpha prerelease.
  To try it: `npm i -D @apleasantview/eleventy-plugin-baseline --legacy-peer-deps`
  then build with `USE_BASELINE=1` (the config loads it lazily and safely).

## Develop

```bash
npm install
npm start        # dev server at http://localhost:8080 (rebuilds + reindexes)
npm run build    # production build to _site/ (runs Pagefind)
```

## Structure

All site source lives under `src/` (the Eleventy input dir). Build tooling
(`eleventy.config.js`, `_config/`, `locales/`) stays at the repo root — config
`.js` files must live outside the input dir or Eleventy treats them as templates.

```
src/                 # Eleventy input dir — everything the site is built from
  posts/*.md           # articles (+ posts.11tydata.js sets layout/permalink/tags)
  _drafts/*.md         # drafts: preview in `npm start`, excluded from `npm run build`
  pages/*.md           # standalone pages (about, contact)
  index.md             # homepage (layout: home.njk)
  latest.njk           # paginated "Latest" archive → /latest/
  authors.njk          # one archive page per author → /author/<slug>/
  tags.njk             # one archive page per topic → /tag/<slug>/
  feed.njk             # Atom feed → /feed.xml
  _includes/
    layouts/           # default, post, page, home
    partials/          # cards, meta, search, theme-toggle, head, icons/…
  _data/               # metadata, navigation, theme, authors (all Pages-CMS editable)
  assets/              # compiled CSS/JS/fonts from the original theme + darkmode
eleventy.config.js     # input=src; collections, drafts preprocessor, Pagefind hook
_config/filters.js     # t (i18n), dates (timezone-aware), reading time, slug, …
locales/en.json        # i18n strings for the `t` filter
.pages.yml             # Pages CMS schema
```

### Drafts
Mark any post a draft with `published: false` in its front matter, or drop it in
`src/_drafts/` (its directory data defaults `published` to false). Drafts render
in `npm start` for preview and are stripped from `npm run build`. Promote a draft
by moving it to `src/posts/` or setting `published: true`.

### Timezone
`_data/metadata.yaml → timezone` (an IANA name like `America/New_York`) sets the
zone for all rendered dates. Front-matter dates are treated as wall-clock time in
that zone, so a `date: 2026-06-12` always shows as *12 Jun 2026* — no UTC drift.

## Features

### Search (Pagefind Component UI)
`_includes/partials/head.njk` loads `pagefind-component-ui.{css,js}`; the header
renders a `<pagefind-modal-trigger>` and the base layout a `<pagefind-modal>`
(open with the button or <kbd>⌘/Ctrl</kbd>+<kbd>K</kbd>). Only article bodies are
indexed — post/page `<article>` elements carry `data-pagefind-body`, so listing
pages are skipped.

### Light / dark mode
An inline no-flash script in `<head>` sets `data-theme` from `localStorage` or the
OS preference before first paint. `assets/css/darkmode.css` remaps the theme's
colour tokens; `assets/js/theme-toggle.js` handles the toggle button and persists
the choice. Default preference: `_data/theme.yaml → default_color_scheme`.

### Pages CMS
Edit content in a browser at [app.pagescms.org](https://app.pagescms.org) — install
the GitHub app on this repo and it reads `.pages.yml`. Posts, pages, authors,
navigation, site settings and theme options are all editable; commits trigger a
rebuild.

## Credits

- **[Eleventy (11ty)](https://www.11ty.dev/)** — the static site generator that builds this site.
- **[Headline](https://github.com/TryGhost/Themes)** — the original theme is the Ghost "Headline" theme by Ghost Foundation (MIT); this repo is an Eleventy conversion of it.
- **Phosphor Icons** — used for the search and light/dark toggle icons. [Phosphor](https://phosphoricons.com/) is a passion project by [Helena Zhang](https://helenazhang.com/) and [Tobias Fried](https://tobiasfried.com/).

## License

[MIT](LICENSE). Headline is © 2013–2026 Ghost Foundation.


## Change Log

### 15 Jul 2026

#### Subs and Podcast Button

Add Button Subs and Podcast 

IntegratoinButton Subs and Podcast `metadata.yaml`

```
subscribe:
  text: Subscribe
  url: "#https://stevennewcomb.substack.com/"
podcast: 
  text: Podcast
  url: "https://dominationchronicles.com/episodes/"
```

#### Fix Structures Loop Grid

image left, title, description, author, date. six most recent posts underneath it.

#### Add new Three Feed menu

Then I want three feeds. if you are looking at the headline.ghost.io page I want getting started to be the https://stevennewcomb.substack.com/feed, science to be https://www.youtube.com/@DominationChronicles, and technology to be https://dominationchronicles.com/feed/feed.xml

metadata.yaml

```
feeds:
  getting_started:
    title: Getting Started
    desc: Subscribe to Steven Newcomb's Substack
    url: https://stevennewcomb.substack.com/feed
    label: RSS Feed
  science:
    title: Science
    desc: Watch Domination Chronicles on YouTube
    url: https://www.youtube.com/@DominationChronicles
    label: YouTube
  technology:
    title: Technology
    desc: Latest episodes from Domination Chronicles
    url: https://dominationchronicles.com/feed/feed.xml
    label: RSS Feed
```

#### Migration WP posts

Complete migration all wp post in to `src/posts`
