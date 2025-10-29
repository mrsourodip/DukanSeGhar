# DukanSeGhar

# 🛒 - Digital Inventory & Delivery Management Platform

A functional prototype inspired by Blinkit — built for **local shopkeepers** to digitize their inventory, manage stock, and assign deliveries to nearby partners.

Unlike typical B2C delivery apps, **LocalStore** focuses purely on the **shopkeeper’s workflow** — from adding loose or packed items to generating bills and coordinating deliveries with local riders.

---

## 🚀 Project Overview

**LocalStore** enables a shopkeeper to:
- Add and manage both **loose items** (e.g., rice per kg, oil per litre) and **packed items** (e.g., salt packets, refined oil).
- Track inventory and stock changes in real-time.
- Create and manage customer orders directly from the shopkeeper dashboard.
- Mark orders as **ready for pickup**, triggering assignment to the nearest delivery partner within 5 km.
- Coordinate delivery via a **React Native app** used by delivery partners.

The goal: help small, local stores go digital — without needing a customer-facing platform.

---

## 🧩 Tech Stack

| Layer | Technologies |
|-------|---------------|
| **Frontend (Web)** | React.js, TypeScript, Axios, Tailwind |
| **Mobile (Delivery App)** | React Native, Expo, FCM |
| **Backend Services** | Node.js (Express), .NET (Delivery Service), Kafka, Redis, PostgreSQL |
| **Infrastructure** | Docker, Kubernetes (GKE), Terraform, GCP Cloud SQL, Memorystore |
| **DevOps** | Helm, GitHub Actions, Cloud Build, Artifact Registry |
| **Monitoring** | Prometheus, Grafana, Cloud Logging |

---

## 🏗️ Architecture Overview
┌────────────────────────────┐
│ Shopkeeper Web │ (React)
└──────────────┬─────────────┘
│ REST API
┌──────────────▼──────────────┐
│ API Gateway │ (Node.js)
└──────────────┬──────────────┘
Kafka Events (Pub/Sub)
┌──────────────┼──────────────┐
│ Orders & Inventory Service │ (Node.js + PostgreSQL)
│ Delivery Service (Assignment│ (Node/.NET + Redis GEO)
│ Billing & Notification Svc │ (Node.js + FCM)
└──────────────┴──────────────┘
│
┌──────▼──────┐
│ PostgreSQL │ (Cloud SQL)
│ Redis Cache │ (Memorystore)
│ Kafka │ (Strimzi / Confluent)
└─────────────┘



---

## 📋 Core Features

### 🏬 Shopkeeper Web App
- Login & role-based access
- Add/update/delete items
- Manage loose (by weight/volume) and packed items
- Track inventory & generate bills
- Create and manage orders

### 🚴 Delivery Partner App (React Native)
- View assigned orders
- Accept / Reject pickups
- Update order status (picked up → delivered)
- Location updates (via Redis GEO)
- Push notifications (via FCM)

### ⚙️ Backend Services
- **Orders & Inventory Service:** Handles items, stock, and order lifecycles.
- **Delivery Service:** Listens to Kafka `order.created` events, assigns partners within 5 km.
- **Billing Service:** Generates order bills and reports.
- **Notification Service:** Sends push and SMS notifications.
- **Redis:** Caching & geo-indexing.
- **Kafka:** Event-driven communication between microservices.

---

## 🧠 Database Schema (PostgreSQL Overview)

- `shops` — Shop details & location  
- `items` — Inventory (loose & packed)  
- `orders` & `order_items` — Order lifecycle  
- `delivery_partners` — Partner details & location  
- `partner_assignments` — Order ↔ Partner mapping  
- `inventory_logs` — Track stock changes  

---

## 🗺️ Event Flow Example

1. Shopkeeper creates an order → `Orders Service` stores and emits `order.created`.
2. `Delivery Service` consumes event, finds nearest partner via Redis GEO, emits `order.assigned`.
3. Partner accepts order in app → emits `order.picked_up`.
4. Partner delivers → `order.delivered` → Billing & notification triggered.

---

## ⚒️ Setup Instructions (Local Development)

### Prerequisites
- Node.js ≥ 18
- .NET 7 SDK (for Delivery Service)
- Docker & Docker Compose
- PostgreSQL 15
- Redis
- Kafka (local or Confluent Cloud)

### Steps

```bash
# 1. Clone repo
git clone https://github.com/<your-username>/localstore.git
cd localstore

# 2. Setup backend services
cd services/orders
npm install

# 3. Run PostgreSQL, Redis, Kafka locally
docker-compose up -d

# 4. Setup environment variables
cp .env.example .env
# edit Postgres, Redis, Kafka creds

# 5. Run backend services
npm run dev

# 6. Start React web app
cd ../../web/shopkeeper
npm install && npm start

# 7. Start React Native app (Expo)
cd ../../mobile/partner
npm install && npx expo start
```
---

🧑‍💻 Team & Responsibilities
Member	            Focus 
Sourodip Kundu	    Node.js backend (Orders/Inventory), React web app, Kafka producers, Redis caching, DB design
Manik	              .NET Delivery Service, React Native mobile app, Kafka consumers, Terraform, Docker/K8s, GCP infrastructure


---

📄 License

MIT © 2025 Sourodip Kundu & Manik



