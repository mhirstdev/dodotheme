# dodo with cap — Micro.blog theme

A minimalist, **dark**, text-first theme for [Micro.blog](https://micro.blog) / Hugo.
Built on top of [theme-blank](https://github.com/microdotblog/theme-blank), so all
Micro.blog plumbing (feeds, archive, photos, JSON Feed, RSD, IndieAuth, plugins,
`custom.css` overrides) keeps working.

Design language:

- **Terminal / mono structure** — monospace for the wordmark, timestamps, labels and
  navigation; a clean sans for reading text.
- **Tonic accent** `#7fb3a4` (a muted sage/teal), used sparingly — timestamps, active
  nav, the `_` cursor, category labels and the pull-quote box.
- **Hairline boxes + rule dividers** instead of heavy chrome. Dark, near-monochrome
  palette (`#0d0d0e` background).

## Content types

| Type | How it's detected | Where it shows |
|------|-------------------|----------------|
| **Kurzmeldung** (micropost) | a post in category `micro` (no title → shown in full) | home `/` (the stream) |
| **Blog post** (article) | a post in category `longpost` (with title → teaser) | `/categories/longpost/` (paginated overview) |
| **Inhaltsseite** (page) | a regular page (e.g. `about`) | its own URL |
| **Galerie** | any post with a `photos` field | `/photos/` (grid) |

Categories are the Micro.blog taxonomy (`categories`). The **structural** categories
`micro` and `longpost` decide where a post appears (home vs the blog overview) and are
hidden from the visible tag lists. **Topical** categories (Festival, Motorsport, …) get
button links at the top of the blog/category pages and their own pages at
`/categories/<name>/`.

## Pages / navigation

The header links to `stream` (`/`), `blog` (`/categories/longpost/`), `galerie`
(`/photos/`) and `about` (`/about/`). Edit `layouts/partials/header.html` to change them.

- `/` — home, lists posts in category `micro` (paginated, 25 per page).
- `/categories/longpost/` — the **blog overview**: Hugo's auto-generated category page
  for `longpost`, paginated (25) with a row of category buttons at the top that link to
  the other category pages. This is the `blog` entry in the nav — no custom page needed.
- `/categories/<name>/` — every category has its own paginated page (same layout).
- `/photos/` and `/archive/` are generated automatically (Micro.blog output formats).

> **Categories drive the split:** the home lists `micro`, the blog lists `longpost`. To
> use different category names, edit the `.Site.GetPage "/categories/…"` lines in
> `layouts/index.html` and `layouts/_default/list.html`, and the `blog` href in
> `layouts/partials/header.html`.
>
> A custom page (`layout: blog`) can't paginate in Hugo, and Micro.blog only creates
> regular pages — that's why the blog overview uses the `longpost` **category page**,
> which paginates natively.

## Customizing

- **Tagline** under the wordmark: set `params.tagline` in your config (shown on home only).
- **Accent / colors**: edit the CSS variables at the top of
  [`static/css/style.css`](static/css/style.css) (`--accent`, `--bg`, …).
- **Fonts**: self-hosted in [`static/fonts/`](static/fonts) — JetBrains Mono (structure)
  + Inter (reading text), `woff2`, latin subset incl. ä ö ü ß. Declared via `@font-face`
  at the top of `static/css/style.css` and wired into `--font-mono` / `--font-sans`. No
  external CDN (GDPR-friendly). To swap a typeface, drop new `woff2` files into
  `static/fonts/` and edit those few lines.
- **User overrides**: anything in Micro.blog's `custom.css` loads *after* the theme CSS,
  so you can tweak without forking.

## Local preview

```bash
hugo server --source exampleSite
```

The `exampleSite/` folder ships with sample microposts, articles, a gallery, an about
page, so you can see every layout immediately.

## Export & install on Micro.blog

A Micro.blog custom theme is just a **public Git repo with this folder at its root**.

**The theme = everything here**, in particular `layouts/`, `static/` (incl. `css/` and
`fonts/`), `config.json`, `theme.toml`, `LICENSE`. Keep `config.json` with its
`[TITLE]`, `[USERNAME]`, … tokens — Micro.blog fills them in at build time. (The filled
`exampleSite/config.json` is for local preview only.)

**Not part of the live site:** `exampleSite/` is preview-only (its content is never
published). `public/`, `exampleSite/public/` and `.hugo_build.lock` are build artifacts
(already in `.gitignore`).

Steps:

1. Make sure nothing unwanted sits in `static/` — every file there is published verbatim
   (e.g. remove `static/blog-theme-drafts.html` unless you want it live).
2. `git init && git add . && git commit -m "dodo with cap theme"`
3. Push to a **new public GitHub repo**.
4. In Micro.blog: **Design → Edit Themes → New external theme**, paste the repo URL,
   then select it as your blog's active theme.
5. Add a page **Blog** with `layout: blog` (see above). Done.

## License

MIT — see [LICENSE](LICENSE). Derived from Micro.blog's theme-blank.
