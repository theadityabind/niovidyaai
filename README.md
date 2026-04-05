<div align="center">

# NIOVIDYA OS
### The AI that learns how you think, not just what you study.
**Built for India's 10M+ JEE, NEET & UPSC students.**

[![Live](https://img.shields.io/badge/Live-niovidya.com-00B4D8?style=for-the-badge)](https://niovidya.com)
[![Stack](https://img.shields.io/badge/Stack-React%2019%20%2B%20Fastify%20%2B%20Supabase-blueviolet?style=for-the-badge)]()
[![AI](https://img.shields.io/badge/AI-Gemini%20Flash%20%2B%20Groq%20Llama-orange?style=for-the-badge)]()

</div>

---

## What is Niovidya?

Most EdTech apps track **what you studied.**
Niovidya tracks **how you think.**

Niovidya is a 7-layer AI operating system that builds a real-time 
model of each student — tracking mastery decay, error patterns, 
forgetting curves, and study behavior — then uses that model to 
make every study session dramatically more efficient.

It is not a study app. It is a student model.

---

## The Problem

Every year, 1.5 million students fail JEE not because they are 
not smart — but because they studied the wrong thing at the 
wrong time with no feedback on why they keep getting it wrong.

Existing solutions:
- **Byju's / Unacademy** → video content, no personalization
- **Khan Academy** → generic learning paths, not exam-specific  
- **ChatGPT** → answers questions, no memory of the student
- **Flashcard apps** → memory only, no decision engine

None of them build a model of the student.

---

## The 7-Layer OS
┌─────────────────────────────────────────┐
│  LAYER 1 — DATA COLLECTION              │
│  Every answer, session, time, emotion   │
├─────────────────────────────────────────┤
│  LAYER 2 — STUDENT MODEL                │
│  Per-topic mastery + forgetting curve   │
├─────────────────────────────────────────┤
│  LAYER 3 — DECISION ENGINE              │
│  Daily plan via priority scoring        │
├─────────────────────────────────────────┤
│  LAYER 4 — NOVA AI (Execution)          │
│  5-mode AI tutor powered by Gemini      │
├─────────────────────────────────────────┤
│  LAYER 5 — EVALUATION ENGINE            │
│  Error classification + mastery update  │
├─────────────────────────────────────────┤
│  LAYER 6 — MEMORY ENGINE                │
│  SM-2 spaced repetition algorithm       │
├─────────────────────────────────────────┤
│  LAYER 7 — ADDICTION ENGINE             │
│  XP + streaks + mountain + missions     │
└─────────────────────────────────────────┘

---

## Nova AI — The 5 Modes

Nova is not a chatbot. It is a multi-mode AI tutor that 
switches behavior based on what the student needs right now.

| Mode | Trigger | Behavior |
|------|---------|----------|
| **Teacher** | Student asks concept | Explains with examples, checks understanding |
| **Tester** | After explanation | Generates targeted question to verify |
| **Challenger** | High mastery detected | Pushes with harder problems |
| **Coach** | Frustration detected | Motivates, reframes, reduces pressure |
| **Analyst** | After a session | Reviews performance, identifies patterns |

Mode is selected automatically using intent detection + 
emotion detection on every message. The student never 
manually switches modes.

---

## AI Pipeline

Every message goes through this pipeline before hitting the LLM:
User message
↓
Intent Detection
(ask_concept / ask_question / casual / help / give_up)
↓
Emotion Detection
(frustrated / confused / confident / bored / neutral)
↓
Mode Selection
(teacher / tester / challenger / coach / analyst)
↓
Response Cache Check
(40% cache hit rate on concept queries → saves cost)
↓
Model Routing
(Flash 80% / Pro 10% / Groq fallback 10%)
↓
Context Builder
(student model + topic + last 8 messages)
↓
Prompt Engine
↓
LLM Response + Cost Tracker

---

## Multi-Model AI Routing

| Model | Use case | Cost per 1K tokens |
|-------|---------|-------------------|
| Gemini 1.5 Flash | 80% of all calls | $0.000075 |
| Gemini 1.5 Pro | Complex analysis | $0.00125 |
| Groq Llama 3.1 | Quota fallback | $0.000005 |

Routing logic considers: user plan (free/pro/power), 
Nova mode complexity, and real-time quota status.
At 40% cache hit rate, effective AI cost drops significantly.

---

## Tech Stack

### Frontend
React 19 + TypeScript
Vite 6
Tailwind CSS v4
Framer Motion + GSAP
Zustand (state management)
Deployed: Vercel Edge CDN

### Backend
Fastify (Node.js) — ultra fast API
TypeScript + Zod validation
API versioning (/api/v1/...)
Rate limiting + auth middleware

### Data Layer
Supabase (PostgreSQL)   — primary database
Redis (Upstash)         — caching layer
BullMQ                  — background job queue
Row Level Security      — data isolation

### AI Layer
Google Gemini 1.5 Flash — primary model
Google Gemini 1.5 Pro   — complex reasoning
Groq Llama 3.1 8B       — fallback model
Custom prompt engine    — per-mode system prompts
Response cache          — Redis-backed
Cost tracker            — per-user daily limits

### Auth & Infrastructure
Clerk — authentication
Vercel — frontend deployment
Railway — backend deployment
Upstash — managed Redis

---

## Project Structure
niovidya/
├── apps/
│   ├── web/                          ← React frontend
│   │   └── src/
│   │       ├── pages/
│   │       │   ├── Dashboard.tsx
│   │       │   ├── Nova.tsx
│   │       │   ├── Flashcards.tsx
│   │       │   ├── Pomodoro.tsx
│   │       │   ├── Roadmap.tsx
│   │       │   └── Analytics.tsx
│   │       ├── components/
│   │       │   ├── os/
│   │       │   │   ├── MountainSystem.tsx
│   │       │   │   ├── EnergySystem.tsx
│   │       │   │   ├── StreakCard.tsx
│   │       │   │   └── DailyPlan.tsx
│   │       │   └── nova/
│   │       │       ├── ChatInterface.tsx
│   │       │       └── ModeIndicator.tsx
│   │       ├── hooks/
│   │       ├── stores/             ← Zustand
│   │       └── lib/
│   │
│   └── api/                        ← Fastify backend
│       └── src/
│           ├── routes/             ← API endpoints
│           ├── services/           ← Business logic
│           ├── engines/            ← Core algorithms
│           ├── ai/                 ← AI pipeline
│           ├── workers/            ← BullMQ jobs
│           └── middleware/
│
├── packages/
│   └── shared/                     ← Shared TypeScript types
│
├── supabase/
│   └── migrations/
│       └── 001_initial_schema.sql  ← Full DB schema
│
└── turbo.json                      ← Monorepo config

---

## Database — 20+ Tables Across 7 Layers
Layer 1: users, subjects, topics
Layer 2: user_topics, student_profiles
Layer 3: daily_plans, goal_roadmaps, roadmap_weeks
Layer 4: conversations, questions
Layer 5: user_answers, study_sessions
Layer 6: flashcard_sets, flashcards
Layer 7: streaks, xp_data, xp_log, achievements,
missions, mountain_state, energy_log
Analytics: events, ai_cost_log

Full schema with indexes and RLS policies:
→ [`supabase/migrations/001_initial_schema.sql`](./supabase/migrations/001_initial_schema.sql)

---

## Unit Economics

| Scale | Monthly Cost | Revenue (8% paid × ₹299) | Gross Margin |
|-------|-------------|--------------------------|-------------|
| 100 DAU | ~$3 | — | — |
| 10,000 DAU | ~$355 | ~$180 | — |
| 100,000 DAU | ~$1,700 | ~$28,700 | **94%** |
| 1,000,000 DAU | ~$13,000 | ~$360,000 | **96%** |

Cost efficiency comes from:
- 40% AI response cache hit rate
- Groq fallback for 80% of simple queries at scale
- Redis caching dashboard (5min) and daily plans (1hr)

---

## Scaling Roadmap
Stage 1: 0 → 10K users     Vercel + Supabase free + Upstash free
Stage 2: 10K → 100K users  Add BullMQ workers + read replicas
Stage 3: 100K → 500K users AWS/GCP migration + custom ML models
Stage 4: 500K → 1M users   Microservices + fine-tuned Llama

---

## 90-Day Build Roadmap

- [x] Monorepo setup (Turborepo)
- [x] Complete database schema (20+ tables, full RLS)
- [x] Redis caching layer
- [x] Fastify backend structure
- [x] React 19 frontend with routing
- [ ] Nova AI pipeline (in progress)
- [ ] Decision engine daily planner
- [ ] SM-2 evaluation engine
- [ ] Addiction engine (XP + streaks + mountain)
- [ ] BullMQ background workers
- [ ] Monetization + paywall gates
- [ ] Beta launch

---

## The Moat

**1. Per-student knowledge graph**
After 30 days of use, the system knows how a student 
thinks better than any tutor. That data cannot be replicated.

**2. Domain depth**
The combination of SM-2 + forgetting curve + exam weightage 
+ error classification is built specifically for Indian 
competitive exams. Not a generic AI wrapper.

**3. Behavioral flywheel**
Every session makes the model smarter. The longer a 
student uses Niovidya, the more irreplaceable it becomes.

---

## Status

**Currently:** Active development  
**Beta launch:** May 2026  
**Target market:** JEE Advanced, JEE Main, NEET, UPSC students  
**Built by:** Aditya Bind — Bengaluru, India  

---

<div align="center">

**[niovidya.com](https://niovidya.com)** 

*Built in Bengaluru. For every student who deserves better.*

</div>
