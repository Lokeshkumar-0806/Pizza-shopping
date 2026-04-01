# Wholesale Dealer Mobile App + WhatsApp Ordering (MVP)

This repository now contains a practical starter blueprint for your idea:

- Dealers/customers can **chat on WhatsApp** and ask for today's available products.
- The system reads the message, finds matching in-stock items, and replies automatically.
- Your team uses a **mobile wholesale app** to quickly post products, prices, and stock to a database.

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
- **Backend:** Node.js (NestJS or Express) or Django/FastAPI
- **Database:** PostgreSQL
- **Cache/queue (optional):** Redis
- **WhatsApp:** Meta WhatsApp Business Platform (Cloud API)
- **Hosting:** AWS / GCP / Azure / Render

## Example WhatsApp Flow

1. Dealer sends: `Need rice and sugar price today`
2. Webhook receives message.
3. Service detects product keywords (`rice`, `sugar`).
4. Backend checks today's active catalog and stock.
5. Bot replies with product cards or formatted text:
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

## Phase 2 Features

- AI intent classification
- Multi-language support
- Dealer-specific pricing tiers
- Payment link generation
- Delivery slot scheduling
- Analytics dashboard

## Security Essentials

- JWT auth for app users
- Role-based access (owner, manager, staff)
- Signed webhook verification for WhatsApp events
- Audit logs for stock/price changes
- Rate limit and abuse protection on inbound messages

## Suggested Build Plan (4 Weeks MVP)

- **Week 1:** DB schema + backend auth + product APIs
- **Week 2:** Mobile admin screens + stock update
- **Week 3:** WhatsApp webhook + auto reply logic
- **Week 4:** End-to-end testing + deployment + pilot with 3–5 dealers

## Files in this repo

- `docs/system-design.md` — detailed architecture and API design
- `docs/db-schema.sql` — starter PostgreSQL schema

If you want, the next step can be generating:
1. React Native starter app,
2. Node.js backend starter,
3. WhatsApp webhook service template.
