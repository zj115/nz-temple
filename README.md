# NZ Temple — Almanac & Temple Services Platform

A full-stack web application for a New Zealand-based Chinese temple. The platform offers a daily Chinese almanac (黄历), a rule-based fortune Q&A assistant, religious service bookings, a merit board, and personal user profiles with BaZi (八字) calculations.

**Live site:** https://nz-temple.vercel.app

## Overview

The application serves the Chinese community in New Zealand, with a focus on solving a small but real problem: standard almanacs are calculated for the Northern Hemisphere, so directional guidance (e.g. 财神方位) is wrong south of the equator. This project applies Southern Hemisphere astronomical corrections and presents the almanac in a way that's accurate for users in NZ.

Around the almanac sit the supporting features the temple's users actually want: ceremony bookings, a small online shop, personal profile storage, and a fortune Q&A that draws on the day's almanac data rather than calling out to an external model.

## Screenshots

![Homepage with merit board](screenshots/homepage-merit-board.jpg)
*Homepage with the public merit board and bilingual navigation.*

![Fortune Q&A](screenshots/ai-fortune-qa.jpg)
*Rule-based fortune Q&A drawing on the current day's almanac data.*

![Course dashboard](screenshots/feng-shui-course-dashboard.jpg)
*Course dashboard for Feng Shui content.*

## Tech Stack

**Frontend** — HTML5, CSS3, and vanilla JavaScript. Canvas API for almanac PNG export. `localStorage` for client-side caching and language preference.

**Backend** — Python serverless functions on Vercel. Flask is used only for local development. `lunar-python` handles Chinese lunar calendar calculations.

**Data & auth** — Supabase (PostgreSQL with row-level security), Supabase Auth for users.

**External services** — AWS S3 for media, Formspree for booking email notifications.

## Key Features

**Almanac system** — Four display modes, Southern Hemisphere astronomical corrections, 12 time periods (时辰) with fortune ratings and activity guidance, and PNG export so users can save the day's almanac to their phone.

**BaZi integration** — Three input paths: calculate from a birth date, enter manually, or pull from a saved profile. Personal fortune overlays on top of the daily almanac.

**Fortune Q&A** — A rule-based answer engine that reads the day's almanac data and produces guidance text. Entirely client-side logic; no third-party model calls.

**Service booking** — Tiered pricing for ritual ceremonies, with email confirmation via Formspree.

**User system** — Supabase Auth, role-based access control, profile-based BaZi storage.

**Merit board** — Public contribution display with automatic name masking, bilingual (Chinese/English).

## Architecture

The frontend is a set of static HTML pages with shared navigation and a bilingual UI that flips through `localStorage`. The backend is eight Python serverless functions on Vercel, each handling a specific calculation or query — daily almanac data, BaZi calculation, personal fortune overlays, Q&A responses, and so on.

Supabase holds user profiles, bookings, inventory, merit board entries, and site settings. Row-level security policies keep personal data scoped to its owner. Media files live in S3.

## Project Structure

```
nz-temple-vercel-static/
├── api/                       # Vercel serverless functions (Python)
│   ├── _almanac.py            # Shared almanac calculation logic
│   ├── day.py
│   ├── bazi.py
│   └── ...
├── calendar_modules/          # Calendar generation utilities
├── docs/                      # Internal documentation
├── images/
├── screenshots/
├── index.html                 # Homepage with merit board
├── calendar.html              # Almanac
├── booking.html               # Service booking
├── wealth-ai.html             # Fortune Q&A
├── login.html
├── profile.html
├── admin.html
├── shop.html
├── courses.html
├── fortune.html
├── southern.html              # Southern Hemisphere explanation page
├── style.css
├── supabase.js
├── app.py                     # Local development server only
├── requirements.txt
└── vercel.json
```

## Setup

Prerequisites: Python 3.8+, a Supabase project, and an S3 bucket for media.

```bash
git clone https://github.com/zj115/nz-temple-vercel-static.git
cd nz-temple-vercel-static
pip install -r requirements.txt
cp .env.example .env   # add AWS credentials
python app.py          # local server at http://127.0.0.1:5001
```

Deploy with `vercel --prod`. The Flask server in `app.py` is only used locally and doesn't affect Vercel deployment.

## Environment Variables

```
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=ap-southeast-2
```

Supabase public keys are configured in `supabase.js` — these are anon keys with restricted permissions, which is the intended use.

## Development Summary

I built this end-to-end. A few notes on the interesting parts:

The Southern Hemisphere correction is the reason this project exists. Standard almanac libraries assume Northern Hemisphere astronomy, so directional guidance comes out wrong south of the equator. I layered corrections on top of `lunar-python` so the daily 财神方位 and related guidance is accurate for NZ users. There's a dedicated page (`southern.html`) explaining this to users who notice the difference.

The fortune Q&A is a rule-based engine, not a generative model. It reads the day's almanac data — auspicious activities, taboo activities, time periods — and assembles guidance text using a small rule set. It's deliberately deterministic so the same question on the same day produces a consistent answer.

The almanac PNG export uses the Canvas API to render the calendar as an image users can save and share. The merit board masks names automatically so contributors can see their own entry without exposing other users' full names.

The backend is split into eight small serverless functions on Vercel so each endpoint has a clear scope. Supabase row-level security policies keep personal data scoped to its owner.

## Security and Privacy

Sensitive credentials are kept out of version control and managed through environment variables. Row-level security policies on Supabase tables restrict personal data to its owner. Names on the merit board are masked automatically before display. Public Supabase keys are anon keys with limited permissions.

## Future Improvements

- Automated tests for almanac calculation accuracy.
- Payment gateway integration for online donations.
- Real-time booking confirmations.
- Expanding the Q&A rule set to cover more question categories.
- Calendar event export (iCal).
- A mobile companion app.

## License

*Shared for portfolio and demonstration purposes only.*
