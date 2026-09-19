# ACEIRT™ Academy — Course Player

HTML5 interactive course packages served via GitHub Pages at **https://courses.aceirt.us**

This repo is the static content host for the ACEIRT™ Academy B2C learning track.
Course delivery is embedded as iframes inside GHL Membership lesson posts.

## Folder Structure

```
/admin-skills/          → Administrative Skills bundle (12 courses)
/career-dev/            → Career Development bundle (17 courses)
/human-resources/       → Human Resources bundle (18 courses)
/sales-marketing/       → Sales & Marketing bundle (coming soon)
/leadership/            → Leadership bundle (coming soon)
/digital-tech/          → Digital & Technology bundle (coming soon)
/train-the-trainer/     → Train the Trainer bundle (coming soon)
```

## How to Add a Course

1. Download the HTML5 zip from GHL Media Storage
2. Extract the zip locally — you'll get a folder (e.g. `business-writing/`)
3. Push that folder to the correct bundle path (e.g. `/admin-skills/business-writing/`)
4. The course is live at: `https://courses.aceirt.us/admin-skills/business-writing/`
5. Embed that URL as an iframe in the matching GHL lesson post

## Course Catalog

See `courses.json` for the full catalog with slugs, GHL IDs, and iframe URLs.

## Architecture

- **This repo**: Static HTML5 course content (GitHub Pages)
- **GHL Memberships**: Enrollment, payments, contacts, lesson post iframes
- **ACEIRT™ Course Player (Vibe app)**: Branded wrapper at courses.aceirt.us/course/[slug]
- **ACEIRT™ University Enterprise Track**: B2B SCORM/LMS delivery (separate system)

---

© 2026 ACEIRT™ Intelligence. All rights reserved.