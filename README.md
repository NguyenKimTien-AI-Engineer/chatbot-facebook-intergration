# MekongAI Social — Facebook Messenger Chatbot Platform

**MekongAI Social** is an enterprise-grade, full-stack platform for building, deploying, and managing AI-powered chatbots on Facebook Messenger. It combines social media integration, intelligent conversation handling, and built-in CRM tooling into a single system — so businesses can automate customer support, drive sales conversations, and operate across multiple Fanpages without stitching together separate tools.

Modern customer engagement on Facebook often means high message volume, repetitive inquiries, and the need for fast, consistent responses around the clock. MekongAI Social addresses this by connecting directly to the Facebook Messenger API, processing incoming messages in real time, and routing them through an AI agent that can answer questions, search product catalogs, retrieve knowledge from uploaded documents, and maintain conversation context. Operators manage everything from a web-based dashboard: connect Fanpages, configure bot identity and workflows, upload knowledge bases, review chat history, and monitor performance metrics — all in one place.

The platform is designed for scalability and multi-tenant operation. A hierarchical user model supports **SuperAdmin**, **White Label**, **Partner**, and **End User** roles, making it suitable both as an in-house solution for a single organization and as a resellable product for agencies or SaaS providers. Subscription tiers, usage limits, and role-based access control are built into the core architecture.

At its heart, the system is built on three pillars:

- **Backend API (FastAPI)** — A high-performance REST API that handles Facebook webhooks and OAuth, bot lifecycle management, CRM operations, document ingestion for RAG (Retrieval-Augmented Generation), user authentication, billing, and system administration. MongoDB serves as the primary datastore; Qdrant powers vector search for knowledge retrieval; AWS S3 handles file and image storage.

- **Frontend Dashboard (Next.js)** — A modern React-based admin interface where users connect Facebook pages, design bot identities and conversation workflows, upload and manage knowledge documents, browse CRM data, and view analytics dashboards with interactive charts and conversation history.

- **AI Agent (LangChain / LangGraph)** — An intelligent message processor that goes beyond simple keyword matching. The agent uses LangGraph for structured reasoning, integrates RAG to answer questions from custom documents, exposes tools for product search and CRM lookups, and maintains multi-turn conversation history so replies stay coherent and context-aware.

Whether you are a business looking to reduce support workload on Facebook, an agency deploying bots for multiple clients, or a platform operator offering white-label chatbot services, MekongAI Social provides the infrastructure, AI capabilities, and management tools needed to run production Messenger bots at scale.

---

## Demo Videos

| Demo | Link |
|------|------|
| Chatbot Messenger | [YouTube](https://www.youtube.com/watch?v=lJ_2IPxw0EQ) |
| Web Admin | [YouTube](https://www.youtube.com/watch?v=3b8gbJLzLYU) |

---

## Overview

**MekongAI Social** brings together the following capabilities into a unified platform:

- **Backend API** (FastAPI) — handles Facebook webhooks, bot management, CRM, knowledge base, and user authentication
- **Frontend Dashboard** (Next.js) — admin UI for bot management, Fanpage connection, analytics, and workflow configuration
- **AI Agent** (LangChain / LangGraph) — automated replies with RAG, product search, and conversation history

### Key Features

| Module | Description |
|--------|-------------|
| **Facebook Messenger Bot** | Receive/send messages via webhook, AI agent powered by LangGraph |
| **Bot Management** | Identity, workflow, and bot deployment to Fanpages |
| **Knowledge Base / RAG** | Upload documents (PDF, Word, Excel), chunking, embedding, vector search |
| **CRM** | Manage companies, contacts, products, warehouses, and orders |
| **Social Media** | Facebook OAuth integration, multi-Fanpage management |
| **Dashboard & Statistics** | Metrics, charts, and conversation history |
| **Multi-tenant** | Hierarchy: SuperAdmin → White Label → Partner → End User |
| **Authentication** | JWT, email verification, password reset |

---

## System Architecture

```
┌─────────────────┐     webhook/OAuth      ┌──────────────────┐
│  Facebook       │ ◄────────────────────► │  FastAPI Backend │
│  Messenger      │                        │  (port 1975)     │
└─────────────────┘                        └────────┬─────────┘
                                                    │
         ┌──────────────────────────────────────────┼──────────────────────────┐
         │                                          │                          │
         ▼                                          ▼                          ▼
┌─────────────────┐                      ┌─────────────────┐        ┌─────────────────┐
│  Next.js UI     │                      │  MongoDB        │        │  Qdrant         │
│  (port 3002)    │                      │  (primary DB)   │        │  (vector search)│
└─────────────────┘                      └─────────────────┘        └─────────────────┘
                                                    │
                                                    ▼
                                          ┌─────────────────┐
                                          │  AWS S3         │
                                          │  (file storage) │
                                          └─────────────────┘
```

---

## Tech Stack

| Layer | Technologies |
|-------|-------------|
| Backend | Python 3.10, FastAPI, Uvicorn, Motor/PyMongo |
| AI/ML | LangChain, LangGraph, OpenAI, HuggingFace embeddings |
| Database | MongoDB (primary), Qdrant (vectors), AWS S3 |
| Frontend | Next.js 16, React 19, TypeScript, Tailwind CSS 4 |
| Auth | JWT (access + refresh), bcrypt |
| Infra | Docker Compose |

---

## Project Structure

```
chatbot-facebook-integration/
├── app.py                  # Backend entry point
├── api/v1/                 # REST API modules
├── bot/                    # Facebook Messenger AI agent
├── configs/                # Configuration & prompts
├── controllers/            # Business logic, RAG, auth, CRM
├── tests/                  # Unit tests
├── ui/                     # Next.js frontend
├── docker-compose.yml
├── Dockerfile
└── requirements.txt
```

### One-time Setup Scripts

| Script | Purpose |
|--------|---------|
| `init_defaults_cli.py` | Initialize default data |
| `init_super_admin_cli.py` | Create SuperAdmin account |
| `init_social_platforms.py` | Seed social media platforms |
| `create_messenger_indexes.py` | Create MongoDB indexes for Messenger |
| `create_product_search_indexes.py` | Create product search indexes |
| `setup_s3_bucket.py` | Configure AWS S3 bucket |

---

## Prerequisites

- **Python** 3.10
- **Node.js** 20+
- **MongoDB** 7+
- **Qdrant** (optional, for RAG vector search)
- System libraries: `tesseract-ocr`, `poppler-utils`, `ffmpeg`, `build-essential`

---

## Installation & Running

### 1. Clone the repository

```bash
git clone <repository-url>
cd chatbot-facebook-integration
```

### 2. Configure environment

```bash
cp .env.example .env
# Edit .env with your actual values
```

Important environment variables:

| Variable | Description |
|----------|-------------|
| `MONGODB_CONNECTION` | MongoDB connection string |
| `OPENAI_API_KEY` | OpenAI API key (required for the bot) |
| `FACEBOOK_CLIENT_ID` / `FACEBOOK_CLIENT_SECRET` | Facebook App credentials |
| `FACEBOOK_VERIFY_TOKEN` | Webhook verification token |
| `FACEBOOK_REDIRECT_URI` | OAuth callback URL |
| `AWS_*` | S3 configuration for file uploads |
| `NEXT_PUBLIC_API_BASE_URL` | Backend URL for the frontend |

### 3. Run with Docker Compose (recommended)

```bash
docker compose up --build
```

| Service | URL |
|---------|-----|
| Backend API | http://localhost:1975 |
| Frontend UI | http://localhost:3002 |
| MongoDB | localhost:27017 |
| API Docs | http://localhost:1975/docs |

### 4. Run locally (development)

**Backend:**

```bash
python3.10 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python app.py
```

**Frontend** (separate terminal):

```bash
cd ui
npm ci
npm run dev
```

**First-time setup** (after MongoDB is running):

```bash
python init_defaults_cli.py init
python init_super_admin_cli.py init-system
python init_social_platforms.py
python create_messenger_indexes.py
python create_product_search_indexes.py
```

---

## API Endpoints

Base URL: `http://localhost:1975/api/v1`

| Prefix | Module |
|--------|--------|
| `/auth` | Registration, login, email verification |
| `/users` | User management |
| `/social` | Facebook OAuth, webhook, Fanpage |
| `/bots` | Bot identity, procedure, deployment |
| `/crm` | CRM (companies, products, orders, etc.) |
| `/knowledge` | Document upload, RAG pipeline |
| `/dashboard` | Dashboard metrics |
| `/statistics` | Analytics |
| `/system` | Notifications, settings |

Health check: `GET /api/v1/health`

SuperAdmin API: `/super-admin/*`

---

## Facebook Messenger Integration

1. Create a [Facebook App](https://developers.facebook.com/) with Messenger permissions
2. Set webhook URL: `{SERVER_ADDRESS}/api/v1/social/webhook`
3. Set `FACEBOOK_VERIFY_TOKEN` to match the token in your Facebook App
4. Configure OAuth redirect: `{SERVER_ADDRESS}/api/v1/social/facebook/callback`
5. Connect your Fanpage via Dashboard → Social

---

## Testing

```bash
# Run all tests
python -m pytest tests/

# Run a specific test
python -m pytest tests/test_search_products_tool.py -v
```

---

## Default Ports

| Service | Port |
|---------|------|
| Backend (FastAPI) | 1975 |
| Frontend (Next.js) | 3002 |
| MongoDB | 27017 |
| Qdrant | 6333 |

---

## License

Proprietary — MekongAI / HueAI. Contact the development team for usage and deployment details.
