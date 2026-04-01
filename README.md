# Wholesale Dealer Mobile App + WhatsApp Ordering (MVP)

This repository contains a practical plan to build your product:

- Dealers/customers can **chat on WhatsApp** and ask for today's products.
- The system reads the message, finds matching in-stock items, and replies automatically.
- Your team uses a **React Native wholesale app** to quickly post products, prices, and stock.

## What you asked for (rephrased)

You want a system where:

1. A wholesaler posts products simply from an app.
2. Dealers use WhatsApp chat to ask what is available today.
3. The chat automatically returns product list/price/availability from your database.

## MVP Architecture

1. **Mobile Wholesale App (Admin side)**
   - Login
   - Add/edit products
   - Set today's availability and price
   - Publish/update stock

2. **Backend API + Database**
   - Stores products, inventory, daily catalog, orders, and chat logs
   - Exposes secure APIs for mobile app and WhatsApp bot

3. **WhatsApp Integration Service**
   - Receives incoming messages via webhook
   - Uses simple intent parsing (or AI/NLP) to identify requested product
   - Queries backend
   - Sends response back in WhatsApp

## Recommended Tech Stack

- **Mobile app:** React Native (Expo-managed workflow for faster MVP delivery)
- **Backend:** Node.js + NestJS (recommended for structure) or Express
- **Database:** PostgreSQL
- **Cache/queue (optional):** Redis
- **WhatsApp:** Meta WhatsApp Business Platform (Cloud API)
- **Hosting:** AWS / GCP / Azure / Render

## How to start working on it (practical)

Use this exact order so you can get a working end-to-end MVP quickly.

### Step 0: Prerequisites

Install:

- Node.js 20+
- npm or pnpm
- PostgreSQL 15+
- Expo Go app on your phone (for quick React Native testing)

### Step 1: Create project folders

```bash
mkdir wholesale-mvp
cd wholesale-mvp
mkdir backend whatsapp-bot
npx create-expo-app@latest mobile-admin -t expo-template-blank-typescript
```

### Step 2: Setup database

1. Create a new PostgreSQL database (example: `wholesale_mvp`).
2. Run schema from this repo:

```bash
psql "$DATABASE_URL" -f docs/db-schema.sql
```

3. Seed a few test products (`rice`, `sugar`, `oil`) and dealer phone numbers.

### Step 3: Build backend first (minimum APIs)

Implement these first endpoints from `docs/system-design.md`:

- `POST /auth/login`
- `GET /products`
- `POST /products`
- `POST /inventory/adjust`
- `POST /catalog/today/publish`
- `GET /catalog/today/search?q=rice`

Definition of done for Step 3:

- React Native app can create products.
- API can return today's active products with price and stock.

### Step 4: Build React Native admin screens

Create these screens in order:

1. Login
2. Product list
3. Product create/edit
4. Inventory update
5. Publish today's catalog

Tips:

- Use React Query for API calls and caching.
- Keep form fields simple in v1 (name, SKU, unit, price, quantity, active_today).

### Step 5: Integrate WhatsApp webhook

1. Create `/webhooks/whatsapp/verify` (GET) for verification.
2. Create `/webhooks/whatsapp/events` (POST) for inbound events.
3. For inbound text:
   - normalize text
   - match words against `product_aliases`
   - query `/catalog/today/search`
   - return formatted reply

### Step 6: End-to-end test (must pass)

- Staff marks products active today.
- Dealer sends WhatsApp message: `Need rice and sugar price today`.
- Bot returns matching items with current price and stock.
- Message logged in `whatsapp_messages` table.

## 7-day execution plan (fast MVP)

- **Day 1:** setup repos + DB + schema + seed data
- **Day 2:** auth + product CRUD
- **Day 3:** inventory + daily catalog endpoints
- **Day 4:** React Native screens (login/product/inventory)
- **Day 5:** WhatsApp webhook + parser
- **Day 6:** end-to-end testing + bug fixes
- **Day 7:** pilot with 2–3 real dealer numbers

## Example WhatsApp Flow

1. Dealer sends: `Need rice and sugar price today`
2. Webhook receives message.
3. Service detects product keywords (`rice`, `sugar`).
4. Backend checks today's active catalog and stock.
5. Bot replies with formatted text:
   - Rice 25kg – $18 – In stock: 42
   - Sugar 5kg – $4.20 – In stock: 110

## Core MVP Features (Build First)

- Admin login
- Product CRUD
- Daily availability toggle (`active_today`)
- Stock update screen
- WhatsApp webhook listener
- Basic message parser for product name matching
- Auto-response templates
- Manual order capture (optional in phase 1)

## Security Essentials

- JWT auth for app users
- Role-based access (owner, manager, staff)
- Signed webhook verification for WhatsApp events
- Audit logs for stock/price changes
- Rate limit and abuse protection on inbound messages

## Files in this repo

- `docs/system-design.md` — detailed architecture and API design
- `docs/db-schema.sql` — starter PostgreSQL schema

If you want next, I can generate:

1. React Native app folder structure + starter screens
2. NestJS backend scaffold + endpoint stubs
3. WhatsApp webhook parser starter implementation
