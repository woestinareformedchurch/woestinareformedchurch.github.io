# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Local Development

```sh
./run                          # serves the site at http://localhost:4000 with live reload, drafts, and future-dated posts
bundle exec jekyll build       # one-time build to _site/
```

## Architecture

This is a **Jekyll static site** (GitHub Pages) for Woestina Reformed Church. There is no JavaScript build process — all front-end assets (Bootstrap, jQuery) are vendored under `vendor/`.

### Adding a new weekly service

The primary recurring task is adding a new Sunday service entry. Each new year requires a new data file at `_data/_YYYY/services.yml`. The structure is:

```yaml
- date: March 8, 2026
  title: Third Sunday in Lent
  mp3: true      # true if audio is available on S3
  bulletin: false # true if bulletin PDF is available on S3
```

The `index.html` homepage automatically displays the **last entry** in `_data/_2026/services.yml` as the current service. The archive accordions in `_includes/archive{YYYY}.html` iterate the full list in reverse order.

### Static assets (audio, bulletins, newsletters)

Audio, bulletin PDFs, and newsletter PDFs are **not stored in this repo**. They are hosted on S3 at `https://s3.amazonaws.com/static.woestinareformedchurch.org` (the `site.static_url` variable from `_config.yml`). URL patterns:

- Audio: `{static_url}/{year}/audio/{YYYY-MM-DD}-WRC-service.mp3`
- Bulletin: `{static_url}/{year}/bulletin/{YYYY-MM-DD}-WRC-bulletin.pdf`
- Newsletter: `{static_url}/{year}/newsletter/{YYYY-MM-DD}-WRC-newsletter.pdf`

### Sign images

`img/latest-sign.jpg` is the image shown on the homepage hero. Each service's sign photo is also stored in `img/{year}/{YYYY-MM-DD}-WRC-sign.jpg`. Updating the homepage sign means replacing or symlinking `img/latest-sign.jpg`.

### Announcement banner

`_data/announce.yml` controls a dismissible banner on the homepage (`enabled: true/false`). Toggle it for audio issues or special announcements.

### Sunday school banner

`_data/sundayschool.yml` similarly controls a Sunday school audio banner (`enabled: true/false`).

### Adding a new year

When a new calendar year starts:
1. Create `_data/_YYYY/services.yml`
2. Create `_includes/archiveYYYY.html` (copy from previous year, update year references)
3. Add `{% include archiveYYYY.html %}` at the top of the archive accordion in `index.html`
4. Update `page.year` front matter in `index.html`
5. Update the newsletter link date in `index.html` front matter (`newsletter:`)
