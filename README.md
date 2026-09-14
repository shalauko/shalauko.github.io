# halauko.eu

Personal site and blog. Jekyll, built natively by GitHub Pages from this branch
(`gh-pages`). No CI, no build step, no dependencies to keep current.

## How a post gets here

Articles are written in the `sh-second-brain` Obsidian vault under
`articles/{topic}/{slug}/`. They are **not** authored in this repo.

1. Finish the article in the vault and set `status: ready`.
2. Set `share: true` in its frontmatter.
3. Publish from Obsidian via the **Enveloppe** plugin.

Enveloppe opens a pull request against this branch. Review it, merge, and GitHub
Pages rebuilds. Nothing reaches the web without `share: true`.

### Frontmatter

```yaml
---
title: Basics of UWB indoor localization systems (indoor RTLS)
description: >-
  One paragraph. Used verbatim as the LinkedIn / OpenGraph preview text.
date: 2024-11-28
tags: [uwb, rtls, localization]
status: ready          # idea | skeleton | drafting | ready | published
math: true             # only when the note contains $$ equations
share: true            # Enveloppe's publish gate
permalink: /blog/uwb-rtls-basics/   # the public URL
---
```

`layout` is not needed — `defaults` in `_config.yml` applies it to everything under
`articles/`. The body starts at `##`; the title comes from frontmatter, so there is no `# H1`.

## Layout: one folder per article

```
articles/{topic}/{slug}/
├── {slug}.md        the article
└── assets/          its diagrams
```

Markdown and assets live together, and this tree is **identical to the vault's**. That is why
Enveloppe needs no destination mapping: "Obsidian Path" mode mirrors notes and attachments
straight across.

Articles are Jekyll *pages*, not posts. Posts would have to live in `_posts`, and Jekyll excludes
every `_`-prefixed directory from the output — so their assets could never sit beside them. A
collection has the same underscore problem, and its `permalink` would apply to static files too
(jekyll#6410). `articles/` has no underscore, so its SVGs are copied verbatim and its `.md` files
are rendered as pages.

The public URL comes from `permalink:` in the frontmatter, so it is independent of where the
file sits. Image links are **vault-root-absolute** (`/articles/uwb/03-rtls/assets/twr.svg`) and
resolve identically in Obsidian and on the web, whatever the permalink says.

There is no RSS feed.

## Math

Equations use `$$ ... $$`. Kramdown converts them to `\[ ... \]` and MathJax 3
renders them in the browser, loaded **only** on pages with `math: true`.

Bare `$...$` is deliberately not an inline delimiter, so prose like "costs $5" is
never mistaken for math. For inline math write `$$E = mc^2$$`.

Rendering math in the browser is a known trade-off of building natively on GitHub
Pages, which cannot run the plugin that would pre-render it. If it becomes a
problem, the content is portable: moving to Eleventy means rewriting templates,
not articles.

## Local preview

```sh
./preview           # http://127.0.0.1:4000
./preview 5000      # different port
```

Builds, watches for changes, and serves. Edits rebuild in about a second —
just reload the browser (there is no live-reload).

`./preview` exists because Jekyll's gemspec hard-depends on `em-websocket`,
which is only used for live-reload but needs a C extension that cannot build
without `ruby-dev`. The script puts the installed gems on the load path
directly and skips RubyGems' dependency activation.

To get the standard toolchain instead:

```sh
sudo apt install ruby-dev
gem install --user-install bundler jekyll
bundle install
bundle exec jekyll serve      # ./preview then becomes redundant
```

Note: Ruby 3.2 ships a `strscan` too old for Liquid 5, so Liquid is pinned to
4.0.4 — which is what Jekyll 4 wants anyway, and what GitHub Pages builds with.

## Diagrams

Sources live beside their output in the vault (`.puml` for PlantUML, drawio for
the rest). Only the exported `.svg` is published. All of them carry a baked-in
white background, so the stylesheet presents them as light figure cards in both
colour schemes.

## Credits

The GitHub and LinkedIn marks in `_includes/icon-*.svg` are Font Awesome Free
7.1.0 icons (CC BY 4.0), inlined from the vendored copy that used to live on the
`local-fa` branch — so the site loads no external CSS, JS or fonts at all.
