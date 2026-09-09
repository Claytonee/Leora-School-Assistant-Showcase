<img src="https://cdn.jsdelivr.net/gh/Claytonee/Leora-School-Assistant-Showcase@main/assets/leora-banner.svg" alt="Leora School Assistant — multi-tenant school-management SaaS"/>

# Leora School Assistant — Technical Showcase

![Next.js 16](https://img.shields.io/badge/Next.js-16-00085B?style=flat-square&logo=nextdotjs&logoColor=17D9F9)
![TypeScript](https://img.shields.io/badge/TypeScript-5-00085B?style=flat-square&logo=typescript&logoColor=17D9F9)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16%2B-00085B?style=flat-square&logo=postgresql&logoColor=17D9F9)
![Prisma](https://img.shields.io/badge/Prisma-ORM-00085B?style=flat-square&logo=prisma&logoColor=17D9F9)
![Tailwind CSS 4](https://img.shields.io/badge/Tailwind_CSS-4-00085B?style=flat-square&logo=tailwindcss&logoColor=17D9F9)
![Multi-Tenant](https://img.shields.io/badge/Architecture-Multi--Tenant-00085B?style=flat-square)
![Render](https://img.shields.io/badge/Deploy-Render%20%2F%20Vercel-00085B?style=flat-square&logo=render&logoColor=17D9F9)

> Multi-tenant school-management SaaS for Tanzanian secondary schools.
> **Status:** in active development (~231,000 lines) · **Role:** architect & sole developer · **Since:** June 2026

This repository is a **technical overview only** — the production codebase is proprietary and kept private. Here is what the system is, how it is engineered, and the constraints it is built around.

## The problem

Tanzanian secondary schools run on paper registers, WhatsApp groups and disconnected spreadsheets: grades are computed by hand against NECTA rules, attendance is a paper register, discipline has no record, fees are tracked in exercise books, and parents hear about problems too late. Existing school software is either imported (wrong curriculum model, USD pricing, no offline reality) or a single-tenant per-school install that nobody maintains.

## The solution

One platform, many schools. Each school is an isolated tenant with its own staff, students, classes, grading scales, branding and billing — while sharing one codebase and one operational model designed around how Tanzanian schools actually work.

**252 database models · 500+ API route handlers · 390+ pages** — Next.js 16, TypeScript, PostgreSQL, Prisma, Tailwind CSS 4.

## Core modules

| Module | What it does |
|---|---|
| 🎓 **Academics & grading** | NECTA-aligned grading system with authority chains (who proposes → who reviews → who approves), exam workflows and report generation |
| 📚 **Curriculum** | TIE-aligned curriculum structures, subject allocations and competency tracking |
| 🗓️ **Attendance** | Daily & per-lesson attendance for students and staff, with trend analytics per class and per student |
| 🛡️ **Discipline** | Structured incident log with severity, follow-up actions and KPI tracking for school leadership |
| 💳 **Finance & billing** | Fee structures, invoices, payments and bursar workflows, plus hotspot/billing integration for school networks |
| 📣 **Communication** | Parent communication, announcements and notification workflows |
| 🧑‍💼 **Portals** | Distinct role experiences: owner, headmaster/admin, academic master, exams officer, HOD, bursar, HR, class teacher, teacher, and student portal |
| 🤖 **AI insights** | Per-module AI insight cards (provider failover: Anthropic → OpenAI) for summaries and anomaly flags |
| 🎨 **Branding & identity** | Per-tenant school branding with role-aware badge tiers and verified identity |

## Architecture

```mermaid
%%{init: { "theme": "base", "themeVariables": { "primaryColor": "#00085B", "primaryTextColor": "#EAF7F3", "primaryBorderColor": "#0E4DFF", "lineColor": "#17D9F9", "secondaryColor": "#0127BC", "tertiaryColor": "#00072D", "clusterBkg": "#00072D", "edgeLabelBackground": "#00072D", "fontSize": "13px" } }}%%
flowchart LR
    B["Browser"] --> P["Portals + Server Components<br/>owner · admin · teacher · student"] --> API["REST route handlers<br/>tenant guard · RBAC"]
    B --> API
    API --> PR[("PostgreSQL + Prisma<br/>252 models")]
    API --> AI["AI failover<br/>Anthropic → OpenAI"]
    API --> CDN["Cloudinary CDN"]
    CK["Clerk"] -. session .-> B
    classDef app fill:#00085B,stroke:#0E4DFF,color:#EAF7F3,stroke-width:1.5px;
    classDef data fill:#00072D,stroke:#17D9F9,color:#EAF7F3,stroke-width:1.5px;
    classDef svc fill:#0127BC,stroke:#17D9F9,color:#EAF7F3,stroke-width:1.5px;
    class P,API app;
    class PR data;
    class CK,AI,CDN svc;
```

**Multi-tenancy** — every query is tenant-scoped at the data-access layer; cross-tenant reads are structurally guarded, not conventionally avoided. Role-based access control separates platform administration from school administration from staff from students, with supplemental roles (class teacher, teacher-on-duty) that activate contextually.

<img src="https://cdn.jsdelivr.net/gh/Claytonee/Leora-School-Assistant-Showcase@main/assets/tenant-isolation.svg" alt="Tenant isolation: one codebase and one database serving separated schools, with cross-tenant reads blocked at the data-access layer" width="100%"/>

**Engineering rules the codebase enforces** — additive-only schema evolution (no destructive migrations, soft deletes only), every API response validated before rendering, secrets never committed, and CI gates that run type-checking and tests before any merge. Screens, dropdowns and layout follow one internal design system so 390+ pages stay consistent.

## Stack

| Layer | Choice |
|---|---|
| Framework | Next.js 16 (App Router, Turbopack), React, TypeScript |
| UI | Tailwind CSS 4, shadcn/ui (Base UI primitives), Recharts, Liquid Glass dashboard system |
| Database | PostgreSQL + Prisma ORM (252 models, additive-only migrations, audit logging) |
| Auth | Clerk |
| AI | Anthropic → OpenAI provider failover |
| Infra | Vercel / Render, Cloudinary CDN, GitHub CI checks |

## Why this project matters

It is designed for the environment it serves: Swahili + English, TZS finance, NECTA/TIE compliance, low-bandwidth schools, and school staff who are not software experts. The goal is 100+ schools on one platform with data isolation they can trust.

---

### © & License

**© 2026 Claytone Curthberth Mhina · Leora Tech Solutions. All rights reserved.**

No license is granted for the artwork, animated banners, diagrams or text in this repository. Copying, modification, redistribution and derivative works are prohibited without written permission. Reach me at **claytonecurth@gmail.com** for licensing or walkthroughs.

<img src="https://cdn.jsdelivr.net/gh/Claytonee/Leora-School-Assistant-Showcase@main/assets/leora-footer.svg" alt=""/>
