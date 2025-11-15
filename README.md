# enterprise-application-LedgerLegends
| Full Name         | ID          |
|-------------------|------------|
| Biruk Dereje       | UGR/6190/15 |
| Bisrat Dereje      | UGR/3229/15 |
| Hailemichael Molla | UGR/3629/15 |
| Kena Ararso        | UGR/9085/15 |

# Project Proposal: Smart Inventory and Sales Management System (AI-Driven)

## Business Problem
Small and medium-sized businesses in Ethiopia often face challenges managing their daily operations efficiently. Common problems include:

- Tracking products and stock accurately.
- Preventing stock shortages or overstocking.
- Managing multiple suppliers and purchase orders.
- Recording sales manually.
- Predicting future demand for products or project materials.

These challenges often result in lost sales, wasted resources, and inefficient procurement. Our project aims to solve these problems by providing an Intelligent Business Management System that integrates inventory, procurement, sales, and AI-driven demand forecasting.

## Core Subdomains
- **Inventory operations** — Keeps quantities accurate so businesses avoid stockouts and overstock.
- **Sales recording** — Captures every sale (manually logged) so owners trust daily revenue figures.
- **Procurement coordination** — Ensures timely purchasing so suppliers, orders, and stock needs stay aligned.

## Generic Subdomains
- **User authentication and authorization** — Controls access and roles so staff permissions stay organized.
- **Alerts & notifications** — Warns about low stock or pending orders.

## Supporting Subdomains
- **Demand forecasting** — Helps plan ahead by predicting what products will be needed.
- **Reporting & dashboards** — Gives summaries for quick decision-making.
- **Basic product catalog** — Stores item details used across sales, stock, and purchasing.

## Bounded Contexts within the Core Domain

### Inventory Control Context
Manages stock levels, tracks incoming products, and flags stockouts or overstock. Focuses on keeping counts accurate.

### Sales Recording Context
Logs each sale manually to maintain daily revenue records. Focuses on trustworthy manual data entry and simple reporting.

### Procurement Context
Handles buying from suppliers and updating stock upon arrival. Focuses on lead time and order timing.

## User Stories

### Story 1: Preventing Stockouts After a Sale
When a sale is logged in the Sales Recording Context, a `SaleCreated` event is published and asynchronously received by Inventory Control, which updates stock and publishes `StockLevelUpdated`. If stock falls below a threshold, `LowStockAlert` is emitted to Procurement, triggering a purchase order. Eventual consistency is maintained through asynchronous messaging (RabbitMQ/Kafka) across Sales Recording, Inventory Control, and Procurement.

### Story 2: Coordinating Purchase Orders
When a delivery arrives, Procurement Context publishes `PurchaseOrderDelivered`, which Inventory Control subscribes to and updates stock levels, emitting `StockReplenished`. Sales Recording receives any `BackorderFulfilled` events asynchronously to update pending sales. This ensures all contexts remain eventually consistent without blocking operations, using asynchronous messaging.

### Story 3: Demand Forecasting for Procurement Planning
Inventory Control publishes `StockDataUpdated` asynchronously, which Demand Forecasting uses to generate `DemandForecastGenerated` predicting future needs. Procurement subscribes and creates `SuggestedPurchaseOrder` events based on the forecast. Eventual consistency is achieved as all contexts process updates asynchronously, keeping stock, sales, and procurement aligned.

## AI Driven Feature

### Demand Forecasting
- Uses historical sales, stock levels, and project usage data.
- Predicts which products or materials will be in high demand.
- Recommends reorder quantities and delivery dates to the Procurement Officer.
- Prevents stock shortages and optimizes procurement costs.
