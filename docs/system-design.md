# System Design: Wholesale Dealer + WhatsApp Product Discovery

## 1) Problem Statement

Enable dealers to ask for products via WhatsApp and receive real-time availability/pricing from a centralized wholesale database managed through a simple mobile app.

## 2) Actors

- **Wholesaler admin/staff:** manages products and stock.
- **Dealer/customer:** asks for products and places inquiry/orders via WhatsApp.
- **System admin:** monitors logs and settings.

## 3) High-Level Components

1. **Mobile Admin App (React Native)**
   - Product CRUD
   - Inventory updates
   - Daily catalog publishing

2. **Backend API**
   - Authentication and RBAC
   - Product catalog service
   - Inventory service
   - Dealer and order service

3. **WhatsApp Bot Service**
   - Inbound webhook endpoint
   - Message parsing + intent mapping
   - Query backend APIs
   - Outbound reply sender

4. **PostgreSQL DB**
   - Source of truth for catalog, stock, orders, users

## 4) Data Flow (WhatsApp request)

1. Dealer sends message to WhatsApp business number.
2. Meta Cloud API posts webhook event to your bot service.
3. Bot validates signature and extracts dealer phone + text.
4. Bot normalizes text and maps product keywords.
5. Bot requests `/catalog/today/search?q=...` from backend.
6. Backend returns matching active products with stock + price.
7. Bot formats response and sends message through WhatsApp API.
8. Conversation entry is saved for analytics/support.

## 5) Suggested API Endpoints (MVP)

### Auth
- `POST /auth/login`
- `POST /auth/refresh`

### Products
- `GET /products`
- `POST /products`
- `PATCH /products/:id`
- `DELETE /products/:id`

### Inventory
- `POST /inventory/adjust`
- `GET /inventory/:productId`

### Daily Catalog
- `POST /catalog/today/publish`
- `GET /catalog/today`
- `GET /catalog/today/search?q=rice`

### Dealers
- `GET /dealers`
- `POST /dealers`

### Orders (optional phase-1)
- `POST /orders`
- `GET /orders?status=pending`

### Webhooks
- `GET /webhooks/whatsapp/verify`
- `POST /webhooks/whatsapp/events`

## 6) Intent Parsing Strategy

### MVP (rule-based)
- Normalize text to lowercase.
- Remove punctuation.
- Match against product aliases table.
- Return top N relevant products.

### Phase 2 (AI-assisted)
- Intent labels: product inquiry, price inquiry, order placement, support.
- Entity extraction: quantity, unit, product variant.

## 7) Non-Functional Requirements

- P95 response for WhatsApp query under 2 seconds.
- High availability for webhook endpoint.
- Idempotent processing for duplicated webhook events.
- Basic observability: request logs + error tracking + metrics.

## 8) Deployment Notes

- Separate services for backend and bot.
- Use HTTPS and secure env vars for API tokens.
- Daily DB backups.
- Staging environment before production.


## 9) Frontend Recommendation

- Build the admin app in **React Native** (preferably with Expo for MVP speed).
- Use React Query for API state syncing and offline-friendly retries.
- Suggested screens: Login, Product List, Product Edit/Create, Inventory Update, Today's Catalog Publish.
