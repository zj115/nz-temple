# NZ Temple - Chinese Almanac & Temple Services Platform

A full-stack web application for a New Zealand-based Chinese temple, featuring an interactive Chinese almanac (黄历) system, AI-powered fortune-telling Q&A, religious service bookings, and user profile management.

---

## Overview

This project serves the Chinese community in New Zealand by providing:

- **Personalized Chinese Almanac** - Daily fortune calendar with Southern Hemisphere astronomical corrections
- **AI Fortune Advisor** - Rule-based Q&A system providing guidance based on traditional Chinese almanac principles
- **Service Booking System** - Online booking for religious ceremonies and offerings
- **Merit Board** - Public display of temple contributions with privacy protection
- **User Profiles** - Personal BaZi (八字) calculation and storage for customized fortune readings

The application addresses the unique needs of Southern Hemisphere users by providing geographically-corrected directional guidance (财神方位) and localized calendar calculations.

---

## Tech Stack

**Frontend**
- HTML5, CSS3, JavaScript (Vanilla JS)
- Responsive design with mobile-first approach
- Canvas API for almanac image generation
- LocalStorage for client-side caching

**Backend**
- Vercel Serverless Functions (Python)
- Flask (local development only)
- RESTful API architecture

**Database & Authentication**
- Supabase (PostgreSQL)
- Supabase Auth for user management
- Row-level security policies

**External Services**
- AWS S3 for media storage
- Formspree for email notifications

**Python Libraries**
- `lunar-python` - Chinese lunar calendar calculations
- Standard library for date/time operations

**Deployment**
- Vercel (frontend + serverless functions)
- Automatic deployments from main branch

---

## Key Features

### 1. Chinese Almanac System
- **Four Display Modes:**
  - Basic Northern Hemisphere almanac
  - Southern Hemisphere corrected almanac (mirrored directional guidance)
  - Personalized Northern Hemisphere (with user BaZi integration)
  - Personalized Southern Hemisphere
- **12 Time Periods (时辰)** with fortune ratings and activity recommendations
- **PNG Export** - Generate downloadable almanac images
- **Daily Updates** - Automatic calculation of auspicious/inauspicious activities

### 2. BaZi (八字) Integration
- Three input methods: birthdate calculation, manual stem-branch input, or profile retrieval
- Personal fortune overlay on daily almanac
- Persistent storage in user profiles

### 3. AI Fortune Q&A
- 13 pre-configured questions across 3 categories
- Rule-based answer generation using daily almanac data
- No external API dependencies - fully client-side logic

### 4. Service Booking
- Ritual ceremony bookings with tiered pricing
- Offering purchases (元宝 - joss paper)
- Email confirmation via Formspree integration
- Special event scheduling (e.g., Qingming Festival ceremonies)

### 5. User System
- Registration and authentication via Supabase Auth
- Profile management with BaZi storage
- Role-based access control (admin dashboard)
- Password reset functionality

### 6. Merit Board
- Public contribution display with name masking
- Auto-scrolling animation
- Bilingual support (Chinese/English)
- Admin-controlled visibility

---

## Architecture

### Frontend Structure
- **Static HTML pages** - Each major feature has a dedicated page
- **Shared navigation** - Consistent header/sidebar across all pages
- **Language toggle** - Chinese/English switching via `localStorage`
- **Supabase client** - Shared authentication state in `supabase.js`

### API Endpoints

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/api/day` | POST | Fetch basic daily almanac data |
| `/api/personal_day_birth` | POST | Generate personalized almanac from birthdate |
| `/api/personal_day_manual` | POST | Generate personalized almanac from manual BaZi input |
| `/api/bazi` | POST | Calculate BaZi from birthdate |
| `/api/wardrobe` | POST | Five-element clothing color recommendations |
| `/api/ai_qa` | POST | AI Q&A response generation |
| `/api/get_video_url` | POST | Generate signed S3 URLs for video content |

### Database Schema

**Tables:**
- `member_profiles` - User information and BaZi data
- `ritual_bookings` - Service booking records
- `yuanbao_inventory` - Offering inventory tracking
- `merit_board` - Public contribution display
- `site_settings` - Key-value configuration storage

See `supabase-setup.sql` for complete schema.

---

## Project Structure

```
├── api/                    # Vercel serverless functions
│   ├── _almanac.py        # Shared almanac calculation logic
│   ├── day.py             # Basic almanac endpoint
│   ├── bazi.py            # BaZi calculation endpoint
│   └── ...
├── calendar_modules/       # Calendar generation utilities
├── images/                 # Application assets
├── screenshots/            # README documentation images
├── index.html              # Homepage with merit board
├── calendar.html           # Almanac system
├── booking.html            # Service booking interface
├── wealth-ai.html          # AI Q&A system
├── login.html              # Authentication
├── profile.html            # User profile management
├── admin.html              # Admin dashboard
├── shop.html               # E-commerce page
├── courses.html            # Course information
├── fortune.html            # Fortune-telling page
├── southern.html           # Southern Hemisphere explanation
├── style.css               # Global styles
├── supabase.js             # Supabase client configuration
├── app.py                  # Local development server (Flask)
├── requirements.txt        # Python dependencies
└── vercel.json             # Vercel deployment configuration
```

---

## Installation and Setup

### Prerequisites
- Python 3.8+
- Node.js (for Vercel CLI, optional)
- Supabase account
- AWS S3 bucket (for video content)

### Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/zj115/nz-temple-vercel-static.git
   cd nz-temple-vercel-static
   ```

2. **Install Python dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your AWS credentials
   ```

4. **Run local development server**
   ```bash
   python app.py
   # Server runs on http://127.0.0.1:5001
   ```

5. **Access the application**
   - Open `http://127.0.0.1:5001` in your browser
   - Note: Port 5001 is used because macOS reserves port 5000 for AirPlay

### Deployment to Vercel

```bash
# Install Vercel CLI (if not already installed)
npm install -g vercel

# Deploy
vercel --prod
```

The `app.py` Flask server is only for local development and does not affect Vercel deployment.

---

## Environment Variables

Create a `.env` file in the root directory:

```env
# AWS S3 credentials (for video content)
AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
AWS_REGION=ap-southeast-2

# Supabase JWT Secret (optional, for token verification)
# SUPABASE_JWT_SECRET=your_jwt_secret
```

**Note:** Supabase public credentials are configured in `supabase.js` and are safe for client-side use.

---

## Screenshots

### Homepage with Merit Board
<img src="./screenshots/homepage-merit-board.jpg" width="100%" alt="Homepage with Merit Board" />

### AI Fortune Q&A System
<img src="./screenshots/ai-fortune-qa.jpg" width="100%" alt="AI Fortune Q&A" />

### Course Dashboard
<img src="./screenshots/feng-shui-course-dashboard.jpg" width="100%" alt="Course Dashboard" />

---

## My Contribution

I developed this full-stack application from scratch, handling:

- **Frontend Development** - Built all HTML/CSS/JS pages with responsive design and bilingual support
- **Backend API Design** - Implemented 8 Python serverless functions for almanac calculations and data processing
- **Database Architecture** - Designed PostgreSQL schema with Supabase integration and row-level security
- **Chinese Almanac Logic** - Integrated `lunar-python` library and implemented Southern Hemisphere astronomical corrections
- **Authentication System** - Configured Supabase Auth with role-based access control
- **Canvas Rendering** - Built almanac image generation system for PNG exports
- **Deployment** - Configured Vercel deployment with serverless function routing

---

## Security and Privacy

- All sensitive credentials are stored in environment variables and excluded from version control
- Supabase Row Level Security (RLS) policies protect user data
- Public API keys in `supabase.js` are anon keys with restricted permissions
- User names on merit board are automatically masked for privacy
- No production secrets or customer data are included in this repository

---

## Future Improvements

- Add automated testing for almanac calculation accuracy
- Implement payment gateway integration for online donations
- Add real-time notifications for booking confirmations
- Expand AI Q&A system with more question categories
- Create mobile app version using React Native
- Add calendar event export (iCal format)

---

## Notes

This repository is shared for portfolio and demonstration purposes. Some business-specific details have been generalized to protect client privacy. The application is deployed and actively used by a New Zealand-based organization.

---

## License

This repository is shared for portfolio and demonstration purposes only.
