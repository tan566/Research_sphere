# ResearchSphere

A comprehensive research-discovery platform that helps researchers, academics, and institutions find conferences, journals, publication calls, PhD/postdoc positions, internships, and research papers worldwide.

## 🎯 What It Does

ResearchSphere aggregates real-time research data from multiple academic APIs **and** scraped websites into one place:

- **Papers** — arXiv, Semantic Scholar, OpenAlex, CrossRef, CORE, DBLP
- **Journals** — CrossRef
- **Conferences** — OpenAlex sources
- **Books & Chapters** — Google Books
- **AI Models** — Hugging Face
- **Project Calls / PhD / Postdoc / Internships** — OpenAlex, CrossRef, plus a **website harvester** that scrapes 80+ research sites (WikiCFP, Call4Paper, Conference Index, Jobs.ac.uk, Indian universities, etc.)

## 🏗️ Architecture

All external calls go **server-side** through an Express proxy, so there are **no CORS errors** and **API keys stay secret**:

```
Browser (React, port 8080)
    ↓  http://localhost:8080/api/*
Express Backend (Node.js, port 5000)
    ↓  direct server-to-server calls (no CORS)
External APIs / Websites
```

- **Frontend**: React 18 + TypeScript + Vite + Tailwind + shadcn/ui + React Query
- **Backend**: Express + Node.js + Axios + xml2js + Cheerio
- **Ports**: Frontend `8080`, Backend `5000` (CORS allows `8080`, `8081`, `5173`)

## 🚀 Quick Start

You must run **two servers** — backend and frontend.

### Option 1: Start script (recommended)
```powershell
powershell -ExecutionPolicy Bypass -File start.ps1
```
This launches the backend on port 5000 and the frontend on port 8080 in separate PowerShell windows.

### Option 2: Manual start

**Terminal 1 — Backend:**
```bash
cd server
npm install
npm run dev        # http://localhost:5000
```

**Terminal 2 — Frontend:**
```bash
npm install
npm run dev        # http://localhost:8080
```

Then open **http://localhost:8080**.  
Backend health check: **http://localhost:5000/health**

## 📡 Features & Data Sources

| Feature | Source |
|---------|--------|
| Papers | arXiv, Semantic Scholar, OpenAlex, CrossRef, CORE, DBLP |
| Journals | CrossRef |
| Conferences | OpenAlex sources |
| Books & Chapters | Google Books |
| AI/ML Models | Hugging Face |
| Project Calls / PhD / Postdoc / Internships | OpenAlex, CrossRef + website harvester (80+ scraped sources) |
| Trending Topics / Stats | Backend (mock) |

All paper/journal/conference calls are proxied through `/api/*` on the backend.

## 🔑 Configuration (You Need to Do This)

Edit **`server/.env`** and add the API keys you want. Most APIs work without keys but are heavily rate-limited.

```env
PORT=5000
NODE_ENV=development

# Optional API keys (get these yourself — see sign-up links below)
SEMANTIC_SCHOLAR_API_KEY=your_key_here
CORE_API_KEY=your_key_here
ELSEVIER_API_KEY=your_key_here
SPRINGER_API_KEY=your_key_here

# CORS — frontend origin
FRONTEND_URL=http://localhost:8080

# Email (contact form) — optional
EMAIL_SERVICE=gmail
EMAIL_USER=
EMAIL_PASSWORD=
COLLABORATION_EMAIL=
```

### Get free API keys

| Key | Sign up | Why add it |
|-----|---------|------------|
| **Semantic Scholar** | https://www.semanticscholar.org/product/api#api-key | Higher rate limit for CS/AI papers |
| **CORE** | https://core.ac.uk/services/api | More open-access paper results |
| **Elsevier / Scopus** | https://dev.elsevier.com/ | Journal impact factors & data |
| **Springer Nature** | https://dev.springernature.com/ | Springer journal metadata (5,000 req/day free) |

Frontend **`.env`** (already configured):
```env
VITE_API_URL=http://localhost:5000/api
```

## 🗂️ Project Structure

```
ResearchSphere/
├── server/                  # Express backend
│   ├── routes/api.js        # All /api/* route handlers
│   ├── services/
│   │   ├── researchService.js   # external API + website scraping logic
│   │   ├── harvest/             # website harvester (sources, store, service)
│   │   ├── assistantService.js  # research assistant
│   │   └── journalMatcherService.js
│   ├── scripts/             # journal-corpus / harvest / smoke-test scripts
│   ├── data/harvest-cache.json  # locally cached harvested data
│   ├── .env                 # backend config + API keys
│   ├── server.js            # Express entry point
│   ├── package.json
│   └── README.md            # backend-specific docs
├── src/                     # React frontend
│   ├── pages/               # Index, Publications, Journals, Conferences, ...
│   ├── services/
│   │   ├── apiClient.ts     # frontend → backend API client
│   │   └── api-types.ts     # shared TypeScript types
│   └── ...
├── index.html               # Vite entry point
├── start.ps1                # one-command startup script
├── .env                     # frontend config (VITE_API_URL)
├── README.md                # this file (single project docs)
├── package.json
└── vite.config.ts           # Vite config (frontend port 8080)
```

## 🛠️ Tech Stack

**Backend:** Express, Node.js, Axios, xml2js, Cheerio, dotenv, Helmet, CORS, Compression  
**Frontend:** React 18, TypeScript, Vite, Tailwind CSS, shadcn/ui, React Query, React Router, Lucide React, next-themes

## 🐛 Troubleshooting

| Problem | Fix |
|---------|-----|
| "Cannot connect to backend" | Make sure `cd server && npm run dev` is running on port 5000 |
| "CORS errors" | Confirm `FRONTEND_URL` in `server/.env` matches your frontend URL (default `http://localhost:8080`) |
| "API returns empty arrays" | Check the backend terminal for error logs; add API keys in `server/.env` for Semantic Scholar / CORE |
| Semantic Scholar returns 0 | Add `SEMANTIC_SCHOLAR_API_KEY` to `server/.env` and restart the backend |
| Website harvest empty/slow | The harvester scrapes 80+ sites; some block scrapers (403/429). That's expected — reliable sources still return data |

## 🚢 Deployment

**Backend** — deploy the `server/` directory to any Node.js host (Railway, Render, Heroku, etc.). Set `server/.env` with your production URL and API keys.

**Frontend** — `npm run build`, then deploy the `dist/` directory to Vercel / Netlify / any static host. Set `VITE_API_URL` to your production backend.

Update `.env` files with production URLs before deploying.

## 📝 Notes

- All agent/work-in-progress markdown docs were consolidated into this single `README.md`.
- If you only add **one** API key, make it **Semantic Scholar** — it gives the biggest improvement for paper search.
- The website harvester warmup runs on backend startup and caches data locally in `server/data/harvest-cache.json`.

---

Built by Tanish — MIT License

