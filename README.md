# JA2 v1.13 Documentation

Community documentation for the **Jagged Alliance 2 v1.13** project, published at
**https://1dot13.github.io/documentation/**

The site covers installation, playing (including the full hotkey reference and 30+
feature guides), a campaign walkthrough, configuration (INI/XML), modding (VFS, XML,
Map Editor, Lua) and development (building the source) — 85+ pages in total. Mechanics
are verified against the current game data and source code; every page lists what it
was checked against in its Sources section.

## Working on the docs

The site is built with [MkDocs](https://www.mkdocs.org/) and the
[Material theme](https://squidfunk.github.io/mkdocs-material/). All content is plain
Markdown in the [`docs/`](docs/) folder; the navigation lives in
[`mkdocs.yml`](mkdocs.yml).

To preview locally:

```bash
pip install -r requirements.txt
mkdocs serve
```

then open http://127.0.0.1:8000/documentation/.

Small fixes are even easier: every page on the site has an edit button that takes you
straight to the file on GitHub.

## Deployment

Pushes to `master` trigger the [deploy workflow](.github/workflows/deploy.yml), which
builds the site and publishes it to GitHub Pages.

> **Note:** the repository's Pages settings must be set to *Build and deployment →
> Source: GitHub Actions* (not "Deploy from a branch").

## Credits

Built on the original starter documentation by **tais** and **Yunotchi**, the
[pbworks wiki](http://ja2v113.pbworks.com/) authors, and the SVN-era documentation
authors (BirdFlu and many others). Maintained by the JA2 v1.13 community —
[The Bear's Pit](https://thepit.ja-galaxy-forum.com).
