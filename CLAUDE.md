# ACEIRT™ Course Player — Vibe App

## What This App Does
This is the **ACEIRT™ Course Player** — a lightweight B2C HTML5 course delivery app.
It wraps interactive HTML5 courses in a branded player and embeds them via iframe into GHL Membership lesson posts.

**This is NOT the ACEIRT™ University Enterprise Track.** That is a separate B2B app.

---

## Brand Config

```
Name:       ACEIRT™ Academy — Course Player
Domain:     courses.aceirt.us
Logo:       https://storage.googleapis.com/msgsndr/SqO7aeEhM6QOT3YoHQNI/media/690f6d6fd7811a1405395e1c.png
Colors:
  Primary:      #102d85  (ACEIRT™ Navy)
  Secondary:    #204176  (Dark Navy)
  Accent:       #0c36e4  (Electric Blue)
  Light Accent: #8ec3df  (Sky Blue)
  Background:   #f5f7f7  (Off White)
  Text:         #1a1a2e
Font: Inter (Google Fonts)
```

---

## AI Gateway (ACEIRT™ IntelliGate)

All AI features in this app use ACEIRT™ IntelliGate:
```
Gateway URL: https://intelligate.aceirt.app/v1
API Key:     [User will provide — stored in .env as INTELLIGATE_API_KEY]
Default Model: gpt-4o-mini
```

---

## Architecture

```
GHL Enrollment → Webhook → Course Player → GitHub Pages iframe
                              ↓
                    courses.aceirt.us/course/[slug]
                              ↓
              iframe: courses.aceirt.us/[bundle]/[slug]/
```

### Course Catalog (Live JSON)
```
https://courses.aceirt.us/courses.json
```
This is the single source of truth. Fetch it at runtime — do not hardcode courses.

### GitHub Pages Base
```
https://courses.aceirt.us/[bundle]/[slug]/index.html
```

---

## App Routes

| Route | Purpose |
|---|---|
| `/` | Landing page — ACEIRT™ Course Player home |
| `/course/[slug]` | Individual course player (iframe wrapper) |
| `/track/[bundle]` | Bundle overview — list all courses in a bundle |
| `/progress` | Learner progress dashboard (future) |
| `/auth/enroll?token=[token]` | GHL enrollment webhook landing |

---

## Core Components

### 1. CoursePlayer (`/course/[slug]`)
- Fetch course metadata from `courses.json` by slug
- Render fullscreen iframe: `src={course.githubPagesUrl}`
- Show: course title, track badge, progress bar (localStorage)
- Controls: fullscreen toggle, back to bundle, mark complete
- Mobile responsive — iframe fills viewport

### 2. BundleOverview (`/track/[bundle]`)
- Filter `courses.json` by bundle
- Show grid of course cards: title, completion status (localStorage)
- Progress ring showing X/Y courses complete
- Each card links to `/course/[slug]`

### 3. LandingPage (`/`)
- ACEIRT™ branding header with logo
- "Platform Active" status indicator
- Link to `app.aceirt.us` Academy portal

### 4. CourseCard Component
```
Props: { slug, title, bundle, ghlModule, githubPagesUrl, completed }
```
- Shows title, module badge, completion checkmark
- On click → navigate to `/course/[slug]`

---

## Progress Tracking (localStorage)

```javascript
// Key pattern
localStorage.setItem(`aceirt_complete_${slug}`, "true")
localStorage.getItem(`aceirt_complete_${slug}`) // "true" | null

// Bundle progress
function getBundleProgress(bundle, courses) {
  const bundleCourses = courses.filter(c => c.bundle === bundle)
  const completed = bundleCourses.filter(c => localStorage.getItem(`aceirt_complete_${c.slug}`) === "true")
  return { total: bundleCourses.length, completed: completed.length }
}
```

---

## GHL Integration

### Enrollment Webhook
When a learner buys a GHL offer, GHL fires a webhook to:
```
POST https://courses.aceirt.us/auth/enroll
Body: { contactId, email, productId, offerId }
```

Map productId to bundle:
```javascript
const PRODUCT_BUNDLE_MAP = {
  "6ac27ec6-eaa7-4bf9-a7a7-5c29bfe8e348": ["admin-skills", "career-dev"],
  "8eb45bc3-e8ef-4a82-a20b-42b92c1dfab5": ["human-resources", "workplace-essentials"],
  "f8d5b938-0ae4-496d-a419-377b13d59036": ["supervisors-managers"],
  "4f007ea7-32c2-4dba-9861-b6a7096c59f1": ["sales-marketing"],
  "270b05b4-543b-4ea1-bee0-2dd502df0255": ["microsoft-office"],
  "2d3ae4c4-cecb-432e-9591-8c78b178a422": ["train-the-trainer"],
}
```

### Iframe Embed Code (for GHL lesson posts)
Each GHL lesson post embeds a course using:
```html
<iframe
  src="https://courses.aceirt.us/course/[slug]"
  width="100%"
  height="700px"
  frameborder="0"
  allowfullscreen
  style="border-radius:12px;"
></iframe>
```

---

## GHL Product → Bundle → Course Mapping

| GHL Track | Product ID | Bundles |
|---|---|---|
| Professional Development Academy | `6ac27ec6-eaa7-4bf9-a7a7-5c29bfe8e348` | admin-skills, career-dev |
| HR & Workplace Excellence Center | `8eb45bc3-e8ef-4a82-a20b-42b92c1dfab5` | human-resources, workplace-essentials |
| Leadership & Management Institute | `f8d5b938-0ae4-496d-a419-377b13d59036` | supervisors-managers |
| Sales, Marketing & Business Growth | `4f007ea7-32c2-4dba-9861-b6a7096c59f1` | sales-marketing |
| Digital & Technology Skills | `270b05b4-543b-4ea1-bee0-2dd502df0255` | microsoft-office (coming) |
| Train the Trainer Certification | `2d3ae4c4-cecb-432e-9591-8c78b178a422` | train-the-trainer (coming) |

---

## Course Catalog Summary (91 courses across 6 bundles)

| Bundle | Folder | Courses | Status |
|---|---|---|---|
| Administrative Skills | `admin-skills/` | 12 | Uploaded to GHL |
| Career Development | `career-dev/` | 17 | Uploaded to GHL |
| Human Resources | `human-resources/` | 18 | Uploaded to GHL |
| Workplace Essentials | `workplace-essentials/` | 14 | Uploaded to GHL |
| Supervisors & Managers | `supervisors-managers/` | 11 | Uploaded to GHL |
| Sales & Marketing | `sales-marketing/` | 19 | Uploaded to GHL |
| Personal Development | `personal-dev/` | TBD | Still uploading |
| Microsoft Office 2016 | `microsoft-office/` | TBD | Still uploading |

**Full catalog with slugs and all metadata:** `https://courses.aceirt.us/courses.json`

---

## GitHub Repo
```
Repo:       https://github.com/aceirt/courses
Live site:  https://courses.aceirt.us
Catalog:    https://courses.aceirt.us/courses.json
```

Course content path: `https://courses.aceirt.us/[bundle]/[slug]/`

---

## Tech Stack

- **Framework:** React (Vite) or vanilla JS — keep it lightweight
- **Styling:** Tailwind CSS or inline CSS — ACEIRT™ brand colors
- **State:** localStorage for progress
- **Data:** Fetch `courses.json` at runtime (no backend needed for phase 1)
- **Hosting:** Served via Vibe AI Studio — custom domain `courses.aceirt.us`

---

## Security Rules

1. No auth required for Phase 1 — courses are served publicly (GHL handles access)
2. Never expose IntelliGate API key client-side — all AI calls via server route
3. Enrollment webhook endpoint must validate GHL signature header
4. CORS: only allow `app.aceirt.us` and `courses.aceirt.us` to iframe embed

---

## Build Phases

### Phase 1 (MVP — Build Now)
- [ ] Root landing page with ACEIRT™ branding
- [ ] Fetch and render `courses.json`
- [ ] `/course/[slug]` player page with iframe
- [ ] `/track/[bundle]` bundle overview page
- [ ] localStorage progress tracking
- [ ] Mobile responsive layout

### Phase 2
- [ ] GHL enrollment webhook handler
- [ ] Learner progress sync to GHL custom field
- [ ] Bundle completion certificate display
- [ ] Search and filter across all 91 courses

### Phase 3
- [ ] ACEIRT™ IntelliGate AI course assistant (sidebar chatbot)
- [ ] Progress analytics dashboard
- [ ] GHL contact auto-tagging on course completion

---

## Kick-Off Prompt

Paste this into the Vibe AI Studio chat after adding this CLAUDE.md:

---

```
Build the ACEIRT™ Course Player — Phase 1 MVP.

Read CLAUDE.md first for full context, brand config, and architecture.

Start with:
1. Root landing page (/) — ACEIRT™ navy background, logo, "Platform Active" badge, link to app.aceirt.us
2. Fetch courses.json from https://courses.aceirt.us/courses.json
3. Course player page (/course/[slug]) — fullscreen iframe wrapper with title bar showing course name + track badge
4. Bundle overview page (/track/[bundle]) — course grid with localStorage completion tracking
5. CourseCard component — title, module badge, completion status

Brand: navy #102d85, electric blue #0c36e4, Inter font, ACEIRT™ logo from CLAUDE.md.
Mobile first. Keep it fast and lightweight.

Do NOT build auth, payments, or AI features yet — Phase 1 only.
```