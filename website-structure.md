# Digital Garden — Website Structure

This document tracks custom components, configuration, and architectural decisions for this Eleventy-based digital garden.

---

## Component System

Components are auto-discovered from `src/site/_includes/components/user/{namespace}/{slot}/`. Any `.njk` file dropped into one of these directories is automatically included in the relevant page section. Files are sorted alphabetically.

**Namespaces:** `common`, `notes`, `index`
**Slots:** `head`, `header`, `beforeContent`, `afterContent`, `footer`
**Additional namespaces:** `filetree` (slots: `beforeTitle`, `afterTitle`), `sidebar` (slots: `top`, `bottom`)

---

## Custom Components

### Comments — Cusdis

**File:** `src/site/_includes/components/user/notes/footer/comments.njk`
**Scope:** Notes pages footer
**Added:** 2026-03-09
**Source:** [Cusdis](https://cusdis.com) (open source, MIT — https://github.com/djyde/cusdis)

Anonymous commenting widget with owner moderation. No authentication required for commenters. Visitors can post comments anonymously; the site owner approves or rejects them via the Cusdis dashboard or email notifications.

**Configuration (`.env`):**
```
CUSDIS_APP_ID=        # Your app ID from cusdis.com — leave empty to disable comments
CUSDIS_HOST=https://cusdis.com   # Override with your self-hosted instance URL
```

**Setup:**
1. Go to [cusdis.com](https://cusdis.com) and create a free account
2. Create a new "website" app in the dashboard
3. Copy the App ID and set `CUSDIS_APP_ID` in `.env`
4. Optionally set up email notifications in the Cusdis dashboard for moderation alerts
5. To self-host: see https://cusdis.com/doc#/self-host — set `CUSDIS_HOST` to your instance

**Behaviour:**
- The component renders only when `CUSDIS_APP_ID` is set
- Passes `page.url` as the unique page identifier so comments are scoped per note
- Inherits `BASE_THEME` (`dark`/`light`) for the widget appearance
- All comments require moderation approval before being publicly visible (controlled in Cusdis dashboard)

**Files modified:**
- `src/site/_data/meta.js` — exposes `meta.cusdisAppId` and `meta.cusdisHost` from env vars
- `.env` — added `CUSDIS_APP_ID` and `CUSDIS_HOST` placeholders

---

## Environment Variables

| Variable | Default | Description |
|---|---|---|
| `THEME` | — | Obsidian theme CSS URL |
| `BASE_THEME` | `dark` | Base UI theme (`dark`/`light`) |
| `dgHomeLink` | — | Show home link |
| `CUSDIS_APP_ID` | _(empty)_ | Cusdis app ID — enables comments when set |
| `CUSDIS_HOST` | `https://cusdis.com` | Cusdis host (override for self-hosted) |
