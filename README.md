# 🛡️ TruthGuard AI — Fake News Detector

**AI-powered multi-agent fake news detection system** using Gemini, Groq, Tavily, and NewsAPI.

[![Deploy Frontend](https://vercel.com/button)](https://vercel.com)
[![Deploy Backend](https://render.com/deploy)](https://render.com)

---

## 🌟 Features

- **5 Specialized AI Agents**: Research, Verification, Bias Detection, Summary, Source Credibility
- **Multi-Modal Input**: Text, URLs (with web scraping), Images (OCR via Tesseract.js)
- **Real AI Models**: Google Gemini 2.0 Flash + Groq LLaMA-3 70B
- **Live Fact Checking**: Tavily Search API + NewsAPI integration
- **Detailed Verdicts**: Fake %, Trust Score, Bias Level, Political Lean, Emotional Manipulation Score
- **OCR Support**: Extract text from screenshots, WhatsApp forwards
- **Tamil Language Support**: OCR and analysis in Tamil + English
- **User Auth**: JWT + Google OAuth
- **PDF Export**: Downloadable fact-check reports
- **Admin Panel**: User management, API key management, analytics
- **Dark/Light Mode**, Responsive Design (Mobile + Desktop)

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────┐
│                    FRONTEND (React)                  │
│   Home → Detector → Analysis Result → Dashboard     │
└─────────────────────┬───────────────────────────────┘
                       │ REST API
┌─────────────────────▼───────────────────────────────┐
│                  BACKEND (Node.js)                   │
│                                                      │
│  ┌──────────────────────────────────────────────┐   │
│  │          AI Agent Orchestrator               │   │
│  │                                              │   │
│  │  ┌──────────┐ ┌────────────┐ ┌───────────┐  │   │
│  │  │ Research │ │Verification│ │   Bias    │  │   │
│  │  │  Agent   │ │   Agent    │ │  Agent    │  │   │
│  │  │ (Tavily) │ │  (Groq)    │ │  (Groq)   │  │   │
│  │  └──────────┘ └────────────┘ └───────────┘  │   │
│  │                                              │   │
│  │  ┌──────────┐ ┌────────────┐                │   │
│  │  │ Summary  │ │  Source    │                │   │
│  │  │  Agent   │ │Credibility │                │   │
│  │  │  (Groq)  │ │   Agent    │                │   │
│  │  └──────────┘ └────────────┘                │   │
│  │                                              │   │
│  │  ┌──────────────────────────────────────┐   │   │
│  │  │      Gemini Deep Analysis Agent      │   │   │
│  │  └──────────────────────────────────────┘   │   │
│  └──────────────────────────────────────────────┘   │
│                                                      │
│  URL Scraper (Cheerio)  │  OCR (Tesseract.js)       │
└──────────────────────────┼──────────────────────────┘
                            │
              ┌─────────────▼──────────────┐
              │      MongoDB Atlas          │
              │  Users │ Analysis │ Trending│
              └────────────────────────────┘
```

---

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- MongoDB Atlas account
- API keys (see below)

### 1. Clone & Install

```bash
git clone https://github.com/yourname/truthguard-ai
cd truthguard-ai

# Backend
cd backend && npm install

# Frontend
cd ../frontend && npm install
```

### 2. Environment Variables

**Backend** — copy `backend/.env.example` to `backend/.env`:

```env
PORT=5000
NODE_ENV=development
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/truthguard
JWT_SECRET=your_32_char_minimum_secret_here
JWT_EXPIRES_IN=7d

# Google OAuth (optional)
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
GOOGLE_CALLBACK_URL=http://localhost:5000/api/auth/google/callback

# AI APIs
GEMINI_API_KEY=your_gemini_api_key
GROQ_API_KEY=your_groq_api_key

# Search APIs
TAVILY_API_KEY=your_tavily_api_key
NEWS_API_KEY=your_newsapi_key

FRONTEND_URL=http://localhost:5173
```

**Frontend** — copy `frontend/.env.example` to `frontend/.env`:

```env
VITE_API_URL=http://localhost:5000/api
```

### 3. Get API Keys

| API | Free Tier | Link |
|-----|-----------|------|
| Gemini | 1M tokens/day free | [aistudio.google.com](https://aistudio.google.com) |
| Groq | 100K tokens/day free | [console.groq.com](https://console.groq.com) |
| Tavily | 1000 searches/month free | [tavily.com](https://tavily.com) |
| NewsAPI | 100 requests/day free | [newsapi.org](https://newsapi.org) |
| MongoDB Atlas | 512MB free | [mongodb.com/atlas](https://mongodb.com/atlas) |

### 4. Run Development

```bash
# Terminal 1 — Backend
cd backend
npm run dev
# API running at http://localhost:5000

# Terminal 2 — Frontend
cd frontend
npm run dev
# App running at http://localhost:5173
```

---

## 📡 API Endpoints

### Analysis
```
POST /api/analysis/text          # Analyze text content
POST /api/analysis/url           # Analyze article URL
POST /api/analysis/image         # Analyze image (OCR)
GET  /api/analysis/trending      # Get trending fake news
GET  /api/analysis/:id           # Get analysis by ID
POST /api/analysis/:id/report    # Report content
```

### Auth
```
POST /api/auth/register          # Register user
POST /api/auth/login             # Login
GET  /api/auth/me                # Get current user
GET  /api/auth/google            # Google OAuth
GET  /api/auth/google/callback   # OAuth callback
```

### User
```
GET  /api/user/history           # Analysis history
GET  /api/user/saved             # Saved reports
POST /api/user/save/:id          # Save report
DELETE /api/user/save/:id        # Unsave report
PUT  /api/user/profile           # Update profile
```

### Admin (requires admin role)
```
GET  /api/admin/stats            # Dashboard stats
GET  /api/admin/users            # All users
PATCH /api/admin/users/:id/ban   # Ban user
PATCH /api/admin/users/:id/unban # Unban user
GET  /api/admin/api-keys         # API key list
POST /api/admin/api-keys         # Add/update API key
GET  /api/admin/reports          # All reports
```

### Reports
```
GET /api/reports/:id/pdf         # Download PDF report
```

---

## 🌐 Deployment

### Frontend → Vercel

```bash
cd frontend
npm run build

# Install Vercel CLI
npm i -g vercel
vercel --prod
```

Set environment variable in Vercel dashboard:
- `VITE_API_URL` = `https://your-backend.onrender.com/api`

### Backend → Render

1. Push code to GitHub
2. Create new **Web Service** on Render
3. Connect your repo, set root to `backend/`
4. Build command: `npm install`
5. Start command: `node src/server.js`
6. Add all environment variables from `.env.example`

---

## 🗂️ Project Structure

```
truthguard/
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   │   ├── Home.jsx          # Landing page
│   │   │   ├── Detector.jsx      # Main analysis UI
│   │   │   ├── AnalysisResult.jsx # Results display
│   │   │   ├── Dashboard.jsx     # User dashboard
│   │   │   ├── AdminPanel.jsx    # Admin control panel
│   │   │   ├── About.jsx
│   │   │   ├── Contact.jsx
│   │   │   ├── Login.jsx
│   │   │   └── Register.jsx
│   │   ├── components/
│   │   │   └── layout/Layout.jsx # Navbar + footer
│   │   ├── store/index.js        # Zustand global state
│   │   ├── services/api.js       # Axios API client
│   │   └── index.css             # Cyber theme styles
│   ├── tailwind.config.js
│   └── vite.config.js
│
└── backend/
    ├── src/
    │   ├── server.js             # Express app
    │   ├── models/index.js       # MongoDB schemas
    │   ├── agents/
    │   │   └── orchestrator.js   # Multi-agent AI system
    │   ├── controllers/
    │   │   ├── analysisController.js
    │   │   ├── authController.js
    │   │   └── adminController.js
    │   ├── services/
    │   │   ├── scraper.js        # Cheerio URL scraper
    │   │   └── ocr.js            # Tesseract OCR
    │   ├── routes/
    │   │   ├── analysis.js
    │   │   ├── auth.js
    │   │   ├── user.js
    │   │   ├── admin.js
    │   │   └── report.js
    │   └── middleware/
    │       ├── auth.js           # JWT protect middleware
    │       └── passport.js       # Passport strategies
    └── render.yaml
```

---

## 🤖 AI Agent Details

### How Analysis Works

1. **Input received** (text / URL / image)
2. If URL → Cheerio scrapes article content
3. If image → Tesseract.js extracts text via OCR
4. **Parallel agent execution:**
   - Research Agent → Tavily + NewsAPI search
   - Bias Agent → Groq LLaMA-3 8B (fast)
5. **Sequential:**
   - Verification Agent → Groq LLaMA-3 70B
   - Gemini Agent → Deep analysis + reasoning
   - Summary Agent → Groq (plain-language summary)
6. **Final verdict calculation** combining all agent scores
7. Result saved to MongoDB, returned to user

### Verdict Scale
| Verdict | Fake % | Meaning |
|---------|--------|---------|
| REAL | 0-15% | Verified true |
| MOSTLY_TRUE | 15-30% | Mostly accurate |
| PARTIALLY_TRUE | 30-50% | Mixed accuracy |
| MISLEADING | 50-70% | Distorted facts |
| FAKE | 70-100% | False information |
| UNVERIFIABLE | — | Cannot be checked |

---

## 🔐 Security

- Helmet.js security headers
- Rate limiting (100/15min general, 20/hour analysis)
- MongoDB injection sanitization
- JWT authentication with 7-day expiry
- Input validation and XSS protection
- File upload validation (images only, 10MB max)

---

## 📱 Supported Platforms

- ✅ Desktop (Windows, Mac, Linux)
- ✅ Mobile (Android, iOS)
- ✅ Tablets
- ✅ Progressive Web App ready

---

## 🌍 Language Support

- English (primary)
- Tamil (OCR + detection)
- More via Gemini's multilingual support

---

## 📄 License

MIT License — free to use and modify.

---

Built with ❤️ to fight misinformation | TruthGuard AI 2024
