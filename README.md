# BOARD

> Internal CRM and order management system with Kanban workflow · Employer: [Sykora](https://github.com/sykora) · Role: IT Manager / lead developer

## Problem

Sykora's kitchen division manages hundreds of custom kitchen orders simultaneously across multiple branches. Orders move through design, production, and delivery stages — each with different owners. The team tracked status in spreadsheets and email threads, leading to lost orders, missed deadlines, and no visibility into the pipeline. Managers couldn't see workload distribution or sales performance without manual counting.

## Approach

Built a full-featured internal CRM with a Kanban board at its center. Each kitchen order is a card that moves through configurable status columns via drag and drop. Three user roles (admin, manager, seller) control what each person can see and do. Added a photo gallery per order (kitchen renders, site photos), activity logging, email notifications, calendar integration, and a statistics dashboard. Exposed a public API with separate API-key authentication for external system integration.

## Stack

- **Backend:** NestJS 10, TypeORM, PostgreSQL 15, JWT authentication
- **Frontend:** React 18, Vite, Tailwind CSS, @dnd-kit (drag and drop), Zustand, Recharts
- **Image processing:** Sharp (thumbnail generation on upload)
- **Email:** Nodemailer (SMTP — invitations, password resets)
- **API docs:** Swagger (auto-generated at `/api/docs`)
- **Deployment:** Docker Compose (3 containers: Postgres, NestJS API, React/Nginx frontend)

## Architecture

```mermaid
graph TB
    subgraph "Frontend - React/Nginx :80"
        UI[Kanban Board] --> DND[@dnd-kit<br/>Drag & Drop]
        GAL[Photo Gallery]
        STATS[Sales Statistics<br/>Recharts]
        CAL[Calendar View]
    end

    subgraph "Backend - NestJS :3000"
        AUTH[JWT Auth<br/>3-tier RBAC]
        MOD[11 Modules]
        PUB[Public API<br/>API-Key Guard]
        IMG[Sharp<br/>Thumbnails]
        MAIL[Nodemailer<br/>SMTP]
    end

    subgraph "Data"
        DB[(PostgreSQL 15)]
        VOL[/Upload Volume/]
    end

    UI --> AUTH
    AUTH --> MOD
    MOD --> DB
    IMG --> VOL
    PUB -->|separate auth| MOD
```

## Key features

| Feature | Detail |
|---|---|
| Kanban board | Drag-and-drop order cards across configurable status columns |
| 3-tier RBAC | Admin (full access), Manager (branch-scoped), Seller (own orders) |
| Photo gallery | Per-order image uploads with Sharp-generated thumbnails |
| Activity log | Full history of status changes, comments, and updates per order |
| Sales statistics | 5 stat endpoints — pipeline value, conversion rates, branch comparison |
| Calendar + Booking | Meeting scheduling and sent-meeting tracking |
| Public API | External integration endpoint with dedicated API-key authentication |
| Email notifications | SMTP-based invitations, password resets, status alerts |

## Scale

- 11 NestJS backend modules
- 13 frontend pages
- ~20 REST API endpoints
- 3-tier role-based access control
- 3-service Docker Compose deployment

## Highlights

- **Most complex project in the portfolio** — full CRUD, real-time Kanban, image processing, email, multi-role auth, public API — all designed, built, and maintained by one developer
- **Drag-and-drop Kanban** with @dnd-kit — orders flow through the pipeline visually, replacing spreadsheet-based tracking
- **Public API module** with a separate authentication mechanism (API keys, not JWT) — designed for future integration with Sykora's ERP and external partners
- **License-key system** — deployment includes a license key file mounted as a read-only volume, enabling per-instance activation

## What's not here

Source is proprietary to Sykora and not published. This repo documents architecture and outcomes only.
