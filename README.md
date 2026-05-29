# SmartNest - AI Intelligent Real Estate Aggregator and Advisor

SmartNest is a full-stack web application that aggregates Melbourne property data and provides AI-powered recommendations, an interactive chatbot advisor, market analytics, and a comprehensive property search platform.

## Architecture Overview

```
SmartNest/
├── frontend/        # React + TypeScript + Tailwind CSS + shadcn/ui
├── backend/         # Hono + tRPC + Drizzle ORM + MySQL
├── ai_service/      # Python FastAPI + AI Recommendation Engine + Chatbot
└── data/            # CSV files and JSON data sources
```

## Features

- **Property Search & Filter** - Search by suburb, price, bedrooms, bathrooms, property type
- **AI Recommendations** - Personalized property recommendations based on user preferences
- **AI Chatbot Advisor** - Real-time chat support for property questions
- **Favourites System** - Save and manage favourite properties
- **Market Analytics** - Suburb comparisons, price distributions, market summaries
- **Admin Dashboard** - Security logs, network monitoring, backup management
- **Privacy Center** - GDPR-compliant data export/delete requests
- **User Authentication** - OAuth 2.0 with role-based access control

## Technology Stack

### Frontend
- React 19 + TypeScript + Vite
- Tailwind CSS + shadcn/ui
- tRPC (end-to-end type safety)

### Backend
- Hono (HTTP framework)
- tRPC (API layer)
- Drizzle ORM + MySQL (database)
- OAuth 2.0 authentication
- Zod validation

### AI Service
- Python FastAPI
- NumPy (analytics)
- Custom recommendation engine
- NLP chatbot with Melbourne property knowledge

## Quick Start

### Prerequisites
- Node.js 20+
- Python 3.10+
- MySQL database (or PlanetScale)

### 1. Install Frontend & Backend Dependencies

```bash
cd /mnt/agents/output/app
npm install
```

### 2. Set Up Environment Variables

The `.env` file is pre-configured. Key variables:

```env
DATABASE_URL=mysql://user:pass@host:port/database
APP_ID=your_app_id
APP_SECRET=your_app_secret
```

### 3. Push Database Schema

```bash
npm run db:push
```

### 4. Seed the Database

```bash
npx tsx db/seed.ts
```

### 5. Install AI Service Dependencies

```bash
cd ai_service
pip install -r requirements.txt
cd ..
```

### 6. Run the Application

Start the backend (includes frontend):

```bash
npm run dev
```

Start the AI service (in a new terminal):

```bash
cd ai_service
uvicorn main:app --reload --port 8000
```

The frontend will be available at `http://localhost:3000`
The AI service will be available at `http://localhost:8000`

## API Endpoints

### Backend (tRPC)

| Router | Endpoints |
|--------|-----------|
| `auth` | `me`, `logout` |
| `properties` | `list`, `byId`, `count`, `suburbs`, `priceRange` |
| `favourites` | `list`, `add`, `remove`, `check` |
| `chat` | `getHistory`, `getUserHistory`, `send`, `clearHistory` |
| `analytics` | `suburbStats`, `propertyTypeDistribution`, `compareSuburbs`, `marketSummary` |
| `privacy` | `requests`, `createRequest` |
| `admin` | `dashboardStats`, `securityLogs`, `networkMetrics`, `backupLogs`, `triggerBackup` |
| `alerts` | `list`, `create`, `update`, `delete` |

### AI Service (FastAPI)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Service info |
| `/health` | GET | Health check |
| `/recommend` | POST | Get property recommendations |
| `/chat` | POST | Chat with AI advisor |
| `/chat/advanced` | POST | Advanced chat with context |
| `/suburbs` | GET | Get suburb data |
| `/market-summary` | GET | Market overview |

## Project Structure

```
app/
├── src/                        # Frontend source
│   ├── components/
│   │   ├── ui/                 # shadcn/ui components
│   │   └── layout/
│   │       └── MainLayout.tsx  # Main app layout
│   ├── pages/
│   │   ├── Home.tsx            # Landing page
│   │   ├── Properties.tsx      # Property listing & search
│   │   ├── PropertyDetail.tsx  # Individual property view
│   │   ├── Recommendations.tsx # AI recommendations
│   │   ├── Favourites.tsx      # Saved properties
│   │   ├── Chat.tsx            # AI chat advisor
│   │   ├── Analytics.tsx       # Market analytics dashboard
│   │   ├── Admin.tsx           # Admin dashboard
│   │   ├── Privacy.tsx         # Privacy & consent center
│   │   ├── Login.tsx           # Authentication
│   │   └── NotFound.tsx        # 404 page
│   ├── hooks/
│   │   └── useAuth.ts          # Authentication hook
│   ├── providers/
│   │   └── trpc.tsx            # tRPC client provider
│   └── App.tsx                 # Routes
├── api/                        # Backend
│   ├── router.ts               # tRPC router registry
│   ├── middleware.ts           # Auth middleware (publicQuery, authedQuery, adminQuery)
│   ├── context.ts              # Request context
│   ├── boot.ts                 # Hono server entry
│   ├── properties-router.ts    # Property endpoints
│   ├── favourites-router.ts   # Favourite endpoints
│   ├── chat-router.ts          # Chat endpoints
│   ├── analytics-router.ts     # Analytics endpoints
│   ├── privacy-router.ts       # Privacy endpoints
│   ├── admin-router.ts         # Admin endpoints
│   ├── alerts-router.ts        # Alert endpoints
│   └── queries/                # Database query functions
│       ├── properties.ts
│       ├── favourites.ts
│       ├── chat.ts
│       ├── analytics.ts
│       ├── privacy.ts
│       ├── security.ts
│       ├── network.ts
│       ├── backup.ts
│       └── alerts.ts
├── db/                         # Database
│   ├── schema.ts               # All table schemas
│   ├── relations.ts            # Table relations
│   └── seed.ts                 # Seed data script
├── ai_service/                 # Python AI service
│   ├── main.py                 # FastAPI app
│   └── requirements.txt        # Python dependencies
├── data/                       # Data files
│   └── properties.json         # Melbourne property data
└── public/                     # Static assets
    ├── hero-bg.jpg
    ├── property-1.jpg
    └── property-2.jpg
```

## Authentication

The app uses OAuth 2.0 via Kimi Authentication. Users can:
- Log in with their Kimi account
- Have automatic role assignment (user/admin)
- Access protected features based on role
- Log out and manage their session

## Admin Access

Admin features include:
- Dashboard overview (properties, network, security stats)
- Security log viewer (failed logins, suspicious activity)
- Network performance monitoring
- Backup management and triggering
- Privacy request management

## Data Sources

The application uses Melbourne-specific real estate data including:
- 25+ Melbourne suburbs with accurate pricing
- Property types: house, apartment, townhouse, unit, villa
- Realistic property features and descriptions
- Suburb-level market analytics

## Non-Functional Requirements

- **Security**: OAuth 2.0, role-based access control, input validation
- **Performance**: API latency < 300ms, optimized database queries
- **Availability**: 99.5% uptime target with health monitoring
- **Accessibility**: WCAG 2.1 AA compliant UI with semantic HTML
- **Usability**: 90%+ of users can find a property within 3 minutes

## Team

| Name | Role |
|------|------|
| Md Aiman Hossain | Authentication, Security & Monitoring |
| Afsana Karim | Aggregation System & Network Performance |
| Muhammad Nehal | Chatbot, Privacy & AI Integration |
| Rifat Ahmed | Frontend Design, Dashboard & Analytics |

## License

This project is for educational purposes as part of a university course.
