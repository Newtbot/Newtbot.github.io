# Noots Blog

A personal blog/notes site for Noot (Ti Seng) to write about tech, homelab, school projects, and CTFs. Built with [Hugo](https://gohugo.io/) using the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

## What this is

- **Static site generator**: Hugo compiles Markdown content into static HTML files.
- **Theme**: [PaperMod](https://github.com/adityatelange/hugo-PaperMod) (pulled in as a Git submodule under `themes/PaperMod`).
- **Language**: English only, but i18n scaffold is available if needed later.
- **Output**: The generated site lives in `public/` (this is what gets deployed).

## Project structure

```
noot-blog/
├── hugo.yaml          # Main config: site title, theme, menu, social links, params
├── content/           # All your written content (Markdown)
│   ├── posts/         # Blog posts (each .md file = one post)
│   ├── archives.md    # "Archive" page (layout: archives)
│   └── search.md      # "Search" page (layout: search)
├── static/            # Files served as-is (images, favicons, etc.)
│   └── img/           # Put images here, reference them in posts
├── layouts/           # Custom overrides for theme templates (currently empty)
├── assets/            # Custom CSS/JS processed by Hugo (currently empty)
├── data/              # Structured data files for templates (currently empty)
├── i18n/              # Custom translation strings (currently empty)
├── themes/
│   └── PaperMod/      # The theme (Git submodule — do not edit directly)
├── example/
│   └── hugo-PaperMod/ # Reference example site for the theme (for guidance)
├── public/            # Generated site output (built, not hand-written)
└── .gitmodules        # Tracks the PaperMod submodule
```

## How to work with it

### Prerequisites
- [Hugo extended](https://gohugo.io/installation/) (v0.165.0+ is known to work here).
- Git (for the theme submodule).

### First-time setup (after cloning)
The theme is a submodule, so initialize it:

```powershell
git submodule update --init --recursive
```

### Local development (preview with live reload)
```powershell
hugo server -D
```
Then open the printed `http://localhost:1313` URL. `-D` also shows drafts.

### Build the production site
```powershell
hugo
```
This regenerates everything in `public/`. The `public/` folder is the deployable site.

### Create a new post
```powershell
hugo new posts/my-post-title.md
```
Then edit the file in `content/posts/`. Set `draft: false` and add a `date` when ready to publish. Front matter example:

```markdown
---
title: "My Post"
date: 2026-08-26T01:12:28+08:00
draft: false
categories: ["homelab"]
tags: ["ansible"]
---
```

Posts support PaperMod features: reading time, code copy buttons, word count, share buttons, breadcrumbs, and post navigation — all enabled in `hugo.yaml` under `params`.

### Adding images
Drop files into `static/img/` and reference them in a post like:
```markdown
![alt text](/img/your-image.png)
```

## Configuration notes (`hugo.yaml`)

- `title` / `baseURL`: site identity. Set `baseURL` before deploying (currently empty).
- `theme: ["PaperMod"]`: uses the PaperMod submodule.
- `languages.en.menu.main`: top nav links (Archive, Search, Tags).
- `params.homeInfoParams`: the text on the homepage intro card.
- `params.socialIcons`: GitHub / LinkedIn icons in the header.
- `params.contact`: email, phone, and various display toggles (reading time, code copy, RSS, word count, etc.).

## Deploying

Hugo produces a fully static site in `public/`. Deploy that folder to any static host (GitHub Pages, Netlify, Cloudflare Pages, etc.). Just remember to set `baseURL` in `hugo.yaml` to your real domain first.

## Common gotchas

- **Don't edit files inside `themes/PaperMod/`** — it's a submodule. To customize, add overrides in `layouts/` or `assets/`.
- **`public/` is generated** — don't hand-edit it; rebuild with `hugo`.
- **Drafts** (`draft: true`) won't appear in a normal `hugo` build; use `hugo server -D` to preview them.
- **If the theme folder is empty** after cloning, run `git submodule update --init --recursive`.
