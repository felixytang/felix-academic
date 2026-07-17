# Academic Website Customization Guide

**Live Site:** https://felixytang.github.io/felix-academic/

This site is a hand-built Jekyll clone of the visual style of [lesommer.github.io](https://lesommer.github.io) (Jekyll's stock "minima" theme): plain Helvetica typography, off-white background, blue links, no cards/shadows/dark-mode, four pages only.

---

## Table of Contents
1. [Quick Start](#quick-start)
2. [File Structure](#file-structure)
3. [Adding New Publications](#adding-new-publications)
4. [Updating Projects](#updating-projects)
5. [Updating Your Profile](#updating-your-profile)
6. [Customizing Appearance](#customizing-appearance)
7. [Local Development](#local-development)
8. [Deployment](#deployment)

---

## Quick Start

### Make a Quick Edit
1. Edit files in your local `felix-academic` folder
2. Run these commands:
   ```bash
   git add -A
   git commit -m "Your update message"
   git push origin master:main
   ```
3. Wait 1-2 minutes for GitHub to rebuild the site

---

## File Structure

```
felix-academic/
├── _config.yml          # Main site configuration (author info, publication categories)
├── _data/
│   └── navigation.yml   # Navigation menu items (About / Contact / Projects / Publications)
├── index.md              # Homepage content (photo + short bio)
├── _pages/
│   ├── about.md          # About page (full bio, education, CV link)
│   ├── contact.md        # Contact page (email, profile links)
│   ├── publications.html # Publications listing logic (loops _publications/)
│   └── projects.html     # Projects listing logic (loops _portfolio/)
├── _publications/        # Individual publication files (data only, no standalone pages)
├── _portfolio/           # Research project files (each gets its own page)
├── images/               # Site images
│   └── projects/         # Project images
├── files/                 # PDFs (CV, papers)
│   └── resume.pdf
├── _layouts/              # default.html (shell), home.html, page.html
├── _includes/             # head.html, header.html, footer.html, social.html
├── _sass/                 # _variables.scss, _base.scss, _layout.scss
└── assets/css/main.scss   # Imports the _sass partials + small custom overrides
```

Note: there are no Talks, Teaching, or CV pages — this was a deliberate simplification to match lesommer's 4-page structure. If you want to bring one back, add a page under `_pages/`, a nav entry in `_data/navigation.yml`, and (if it needs its own content type) a collection in `_config.yml`.

---

## Adding New Publications

### Step 1: Create a new file
Create `_publications/YYYY-MM-DD-short-title.md`:

```markdown
---
title: "Your Paper Title"
collection: publications
category: manuscripts
excerpt: 'Brief description of the paper.'
date: YYYY-MM-DD
venue: 'Journal Name'
paperurl: 'https://doi.org/...'
citation: 'Author1, Author2. (YYYY). "Title." <i>Journal</i>.'
---
```

The `citation` field is what actually renders on the Publications page (as a formatted list item) — keep it as one clean HTML/Markdown string. `paperurl` is optional; if set, a `[pdf]` link is added next to the citation. The publications collection does **not** generate individual pages (`output: false` in `_config.yml`), matching lesommer's plain citation-list style — so `permalink` isn't needed.

If you introduce a new `category` value, add a matching entry under `publication_category` in `_config.yml` so it gets its own `<h2>` heading on the Publications page.

### Step 2: Commit and push
```bash
git add _publications/
git commit -m "Add new publication: Paper Title"
git push origin master:main
```

---

## Updating Projects

### Edit existing project
Edit files in `_portfolio/`:
- `project-1-arctic.md`
- `project-2-scs.md`
- `project-3-coastal.md`

### Add a new project
Create `_portfolio/project-4-newname.md`:

```markdown
---
title: "Project Title"
excerpt: "One-line description shown in the Projects list."
collection: portfolio
---

![Project Image]({{ "/images/projects/your-image.jpg" | relative_url }})

## Overview
Project description...

## Methods
- Method 1
- Method 2

## Related Publications
1. Citation 1
2. Citation 2
```

Each portfolio entry gets its own page (unlike publications) since these are fuller write-ups, not one-line citations. Always use the `{{ "..." | relative_url }}` filter for image paths rather than hardcoding `/felix-academic/...` — it keeps links correct if the `baseurl` in `_config.yml` ever changes.

### Add project images
Place images in `images/projects/` and reference them with the `relative_url` filter as shown above.

---

## Updating Your Profile

### Basic Info (`_config.yml`)
```yaml
author:
  name: "Your Name"
  bio: "Your tagline"
  location: "City, State"
  employer: "University"
  email: "your@email.edu"
  googlescholar: "https://scholar.google.com/..."
  researchgate: "https://www.researchgate.net/..."
  github: "username"
  linkedin: "username"
```

### Homepage (`index.md`) and About page (`_pages/about.md`)
Edit the markdown content directly — both are plain prose, the same way lesommer's site is authored (no data-driven templating for bio text).

### Contact page (`_pages/contact.md`)
Plain markdown list — edit directly. Only include contact details you actually want public.

### Profile Photo
Replace `images/profile.png` with your photo (keep the same filename).

### CV PDF
Replace `files/resume.pdf` — it's linked from the About page.

---

## Customizing Appearance

### Colors (`_sass/_variables.scss`)
```scss
$text-color:          #111;
$background-color:    #fdfdfd;
$link-color:          #2a7ae2;
$link-visited-color:  #1756a9;
$grey-color:          #828282;
$border-color:        #e8e8e8;
```

### Layout width (`_sass/_variables.scss`)
```scss
$content-width: 800px;  // matches lesommer's centered column
```

### Custom CSS (`assets/css/main.scss`)
Add custom styles after the `@import` lines at the top of this file.

---

## Local Development

### Setup (one-time)
```bash
# Install Ruby (if not installed)
brew install ruby

# Install dependencies
cd felix-academic
bundle install
```

### Run locally
```bash
bundle exec jekyll serve -l -H localhost
```
Open: http://localhost:4000/felix-academic/

### Stop server
Press `Ctrl+C`

---

## Deployment

### Automatic (recommended)
Just push to GitHub - the site rebuilds automatically:
```bash
git add -A
git commit -m "Update description"
git push origin master:main
```

### Check deployment status
```bash
gh run list --repo felixytang/felix-academic --limit 1
```

---

## Common Tasks

### Add a new navigation item
Edit `_data/navigation.yml`:
```yaml
main:
  - title: "New Page"
    url: /newpage/
```

### Change site title
Edit `_config.yml`:
```yaml
title: "Your Name"
description: "Your description"
```

---

## Troubleshooting

### Site not updating?
- Wait 2-3 minutes for GitHub Actions to complete
- Check: https://github.com/felixytang/felix-academic/actions

### Images not showing?
- Use `{{ "/images/..." | relative_url }}` rather than a hardcoded path
- Check the file exists in `images/`

### Local server errors?
```bash
bundle update
bundle exec jekyll serve
```

---

## Resources

- [lesommer.github.io](https://lesommer.github.io) — the site this design is modeled on
- [Jekyll's minima theme](https://github.com/jekyll/minima) — the upstream theme this site's `_sass`/`_layouts`/`_includes` reimplement by hand
- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [Markdown Guide](https://www.markdownguide.org/)

---

*Last updated: 2026-07-17.*
