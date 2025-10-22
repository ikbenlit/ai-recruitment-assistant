# 🚀 Mission Control – Bouwplan AI Recruitment Assistant

**Projectnaam:** AI Recruitment Assistant  
**Versie:** v1.0  
**Datum:** 17-10-2025  
**Auteurs:** Colin Lit (Lead - Backend/Infrastructure) & Eelco (Frontend/UI)

---

## 1. Doel en context

🎯 **Doel:**  
Een werkend MVP bouwen van de AI Recruitment Assistant dat HR-afdelingen helpt bij het screenen en selecteren van kandidaten. Het prototype toont hoe AI cv-review, matching en rapportage automatiseert en visualiseert in een professioneel dashboard.

📘 **Toelichting:**  
Dit project demonstreert de waarde van AI in HR-processen. We bouwen een showcase die in 10 minuten de volledige workflow toont: cv-upload → AI-analyse → matchscore → dashboard → PDF-export. Het prototype dient als bewijs voor management en klanten.

**Referenties:**
- [PRD – AI Recruitment Assistant](./ai_recruitment_prd.md)  
- [FO – Functioneel Ontwerp](./ai_recruitment_fo.md)  
- [TO – Technisch Ontwerp](./ai_recruitment_technisch_ontwerp_to_draft.md)

---

## 2. Uitgangspunten

**Stack & Technologie:**
- **Frontend:** Next.js 15 (App Router) + TypeScript + Tailwind CSS + shadcn/ui
- **Backend:** Next.js API Routes + Prisma ORM + Neon Postgres (EU-FRA)
- **Workflow:** n8n (EU-hosted) voor AI-orchestration
- **AI:** OpenAI API (structured JSON output)
- **Storage:** Vercel Blob of Supabase Storage voor PDF's
- **Hosting:** Vercel (EU region)
- **Observability:** Sentry + PostHog

**Beperkingen:**
- **Tijd:** 3-4 weken voor MVP
- **Team:** 2 developers (Colin: Frontend, Eelco: Backend)
- **Data:** Alleen fictieve demo-data, geen echte persoonsgegevens
- **Scope:** Focus op CV-review + Matching + Dashboard (geen ATS-integratie)

**Aannames:**
- Demo moet volledig offline kunnen draaien (met seed-data)
- Minimaal 5 test-cv's en 2 vacatures beschikbaar
- AI-output moet consistent en reproduceerbaar zijn

---

## 3. Fase-overzicht

| Fase | Titel | Doel | Verantwoordelijk | Status | Opmerkingen |
|------|--------|------|------------------|---------|--------------|
| 0 | Setup & Configuratie | Repo, omgeving, dependencies | Colin + Eelco | ⏳ To Do | Samen starten |
| 1 | Database & Data Model | Schema, migrations, seed-data | **Colin** | ⏳ To Do | Blokkert fase 2 & 3 |
| 2 | UI Foundation | Layout, componenten, navigatie | **Eelco** | ⏳ To Do | Parallel aan fase 1 |
| 3 | Backend API & n8n | API routes, webhooks, n8n flows | **Colin** | ⏳ To Do | Blokkert fase 4 |
| 4 | AI Integration | Prompts, JSON schemas, testing | **Colin** | ⏳ To Do | Parallel aan fase 5 |
| 5 | Frontend Features | Dashboard, upload, weergave | **Eelco** | ⏳ To Do | Wacht op fase 3 API's |
| 6 | Rapportage & Export | PDF-generatie, templates | **Eelco** | ⏳ To Do | Laatste feature |
| 7 | Integratie & Testing | End-to-end flows, bugfixes | Colin + Eelco | ⏳ To Do | Samen testen |
| 8 | Demo-prep & Deploy | Vercel deployment, demo-scenario | Colin + Eelco | ⏳ To Do | Live gaan |

---

## 4. Subfases (uitwerking per fase)

### Fase 0 — Setup & Configuratie
**Verantwoordelijk:** Colin + Eelco (parallel)  
**Doel:** Development environment opzetten

| Subfase | Doel | Verantwoordelijk | Status | Afhankelijkheden | Opmerkingen |
|----------|------|------------------|--------|------------------|--------------|
| 0.1 | Repo initialiseren | Colin | ⏳ | — | Next.js + TypeScript template |
| 0.2 | Dependencies installeren | Colin | ⏳ | 0.1 | Tailwind, shadcn, Prisma, etc. |
| 0.3 | Environment variables setup | Colin | ⏳ | 0.2 | `.env.local` + Vercel vars |
| 0.4 | Database connection | Colin | ⏳ | 0.3 | Neon Postgres EU, Prisma init |
| 0.5 | n8n environment setup | Colin | ⏳ | 0.3 | EU-hosted instance + webhooks |
| 0.6 | Vercel project setup | Eelco | ⏳ | 0.2 | Link repo, configure build |
| 0.7 | Code style & lint config | Eelco | ⏳ | 0.2 | ESLint, Prettier, Git hooks |

---

### Fase 1 — Database & Data Model
**Verantwoordelijk:** Colin  
**Doel:** Database schema opzetten en seed-data prepareren

| Subfase | Doel | Status | Afhankelijkheden | Opmerkingen |
|----------|------|--------|------------------|--------------|
| 1.1 | Prisma schema definiëren | ⏳ | 0.4 | Tables: vacancies, candidates, reviews, matches |
| 1.2 | Migrations aanmaken | ⏳ | 1.1 | `prisma migrate dev` |
| 1.3 | Seed script schrijven | ⏳ | 1.2 | 2 vacatures + 5 test-kandidaten |
| 1.4 | Database indexen optimaliseren | ⏳ | 1.2 | Indexen op match_score, created_at |
| 1.5 | Audit logs table | ⏳ | 1.2 | Voor tracking demo-acties |
| 1.6 | Seed data uitvoeren & testen | ⏳ | 1.3 | Valideer relations en data |

---

### Fase 2 — UI Foundation
**Verantwoordelijk:** Eelco  
**Doel:** Basisstructuur interface en componenten

| Subfase | Doel | Status | Afhankelijkheden | Opmerkingen |
|----------|------|--------|------------------|--------------|
| 2.1 | Layout skeleton bouwen | ⏳ | 0.6 | Header, sidebar, main content |
| 2.2 | shadcn/ui componenten integreren | ⏳ | 2.1 | Button, Card, Table, Dialog, etc. |
| 2.3 | Navigatie structuur | ⏳ | 2.1 | `/dashboard`, `/vacancies`, `/candidates` |
| 2.4 | Design system setup | ⏳ | 2.2 | Kleuren, typografie, spacing |
| 2.5 | Responsive layout testen | ⏳ | 2.1 | Desktop + tablet views |
| 2.6 | Loading states & skeletons | ⏳ | 2.2 | Shimmer effects voor data laden |
| 2.7 | Error boundaries | ⏳ | 2.1 | Fallback UI voor crashes |

---

### Fase 3 — Backend API & n8n
**Verantwoordelijk:** Colin  
**Doel:** API endpoints en n8n workflows implementeren

| Subfase | Doel | Status | Afhankelijkheden | Opmerkingen |
|----------|------|--------|------------------|--------------|
| 3.1 | Upload API route | ⏳ | 1.6 | `POST /api/upload` + storage |
| 3.2 | Vacancies CRUD API | ⏳ | 1.6 | GET, POST `/api/vacancies` |
| 3.3 | Dashboard API | ⏳ | 1.6 | Aggregated metrics endpoint |
| 3.4 | n8n webhook security | ⏳ | 0.5 | HMAC verification + secret header |
| 3.5 | n8n Flow A: CV Parse → Review | ⏳ | 3.4 | PDF text extraction + DB write |
| 3.6 | n8n Flow B: Vacancy + CV Match | ⏳ | 3.4 | Comparison logic + match write |
| 3.7 | Webhook callback endpoint | ⏳ | 3.4 | `POST /api/webhooks/n8n` |
| 3.8 | Error handling & retries | ⏳ | 3.7 | Exponential backoff in n8n |

---

### Fase 4 — AI Integration
**Verantwoordelijk:** Colin  
**Doel:** OpenAI prompts en JSON schemas implementeren

| Subfase | Doel | Status | Afhankelijkheden | Opmerkingen |
|----------|------|--------|------------------|--------------|
| 4.1 | OpenAI API setup | ⏳ | 0.3 | API key + structured output config |
| 4.2 | Reviewer prompt ontwikkelen | ⏳ | 4.1 | System prompt + JSON schema |
| 4.3 | Matcher prompt ontwikkelen | ⏳ | 4.1 | Comparison logic + scoring |
| 4.4 | JSON schema validatie | ⏳ | 4.2, 4.3 | Zod schemas voor type-safety |
| 4.5 | Prompt testing met test-data | ⏳ | 4.2, 4.3 | Consistentie checken (5+ runs) |
| 4.6 | Model metadata logging | ⏳ | 4.1 | Tokens, latency, model version |
| 4.7 | Temperature & token tuning | ⏳ | 4.5 | Optimaliseer voor consistentie |

---

### Fase 5 — Frontend Features
**Verantwoordelijk:** Eelco  
**Doel:** Interactieve features en data-weergave

| Subfase | Doel | Status | Afhankelijkheden | Opmerkingen |
|----------|------|--------|------------------|--------------|
| 5.1 | Dashboard overview pagina | ⏳ | 2.7, 3.3 | Metrics cards + stats |
| 5.2 | Vacatures lijst & detail | ⏳ | 3.2 | Table met filters + modal |
| 5.3 | CV upload interface | ⏳ | 3.1 | Drag & drop + file validation |
| 5.4 | AI Review resultaten weergave | ⏳ | 3.5 | Score cards + samenvatting |
| 5.5 | Match resultaten dashboard | ⏳ | 3.6 | Sorteerbare tabel met highlights |
| 5.6 | Kandidaten profiel pagina | ⏳ | 3.3 | Detail view + timeline |
| 5.7 | Filters & zoek functionaliteit | ⏳ | 5.2, 5.5 | Score range, naam, skills |
| 5.8 | Real-time status updates | ⏳ | 3.7 | Polling of websocket voor n8n |

---

### Fase 6 — Rapportage & Export
**Verantwoordelijk:** Eelco  
**Doel:** PDF-generatie en management rapportages

| Subfase | Doel | Status | Afhankelijkheden | Opmerkingen |
|----------|------|--------|------------------|--------------|
| 6.1 | PDF template ontwerpen | ⏳ | 5.5 | Logo, styling, layout |
| 6.2 | @react-pdf/renderer setup | ⏳ | 6.1 | Server-side rendering |
| 6.3 | Rapportage data aggregatie | ⏳ | 3.3 | Top kandidaten, avg scores, gaps |
| 6.4 | PDF API endpoint | ⏳ | 6.2, 6.3 | `POST /api/export/pdf` |
| 6.5 | Download functionaliteit | ⏳ | 6.4 | Button in dashboard |
| 6.6 | Email rapportage (stretch) | ⏳ | 6.4 | Optioneel: Resend/SendGrid |

---

### Fase 7 — Integratie & Testing
**Verantwoordelijk:** Colin + Eelco  
**Doel:** End-to-end flows testen en bugs fixen

| Subfase | Doel | Verantwoordelijk | Status | Afhankelijkheden | Opmerkingen |
|----------|------|------------------|--------|------------------|--------------|
| 7.1 | Flow A: Upload → Review | Colin | ⏳ | Alle vorige | 5 test-cv's doorlopen |
| 7.2 | Flow B: Vacature → Match | Colin | ⏳ | Alle vorige | 2 vacatures testen |
| 7.3 | Flow C: Dashboard → Export | Eelco | ⏳ | Alle vorige | PDF valideren |
| 7.4 | UI/UX smoke tests | Eelco | ⏳ | 5.8 | Alle knoppen & navigatie |
| 7.5 | Error scenario's testen | Colin | ⏳ | 3.8 | Upload fails, API timeouts |
| 7.6 | Performance metingen | Beide | ⏳ | 7.1, 7.2 | <60s voor 10 cv's |
| 7.7 | Security audit | Colin | ⏳ | 3.4 | HMAC, env vars, rate limits |
| 7.8 | Cross-browser testing | Eelco | ⏳ | 7.4 | Chrome, Safari, Firefox |
| 7.9 | Bugfix sprint | Beide | ⏳ | 7.1-7.8 | Prioritized bug list |

---

### Fase 8 — Demo-prep & Deploy
**Verantwoordelijk:** Colin + Eelco  
**Doel:** Production deployment en demo-scenario voorbereiden

| Subfase | Doel | Verantwoordelijk | Status | Afhankelijkheden | Opmerkingen |
|----------|------|------------------|--------|------------------|--------------|
| 8.1 | Production env vars | Colin | ⏳ | 7.9 | Vercel + Neon + n8n prod |
| 8.2 | Database migrations prod | Colin | ⏳ | 8.1 | `prisma migrate deploy` |
| 8.3 | Seed prod database | Colin | ⏳ | 8.2 | Demo-data klaarzetten |
| 8.4 | Vercel deployment | Eelco | ⏳ | 8.1 | EU region, custom domain |
| 8.5 | Sentry + PostHog setup | Colin | ⏳ | 8.4 | Error tracking + analytics |
| 8.6 | Demo-scenario schrijven | Eelco | ⏳ | 8.3 | 10-min script met screenshots |
| 8.7 | Dry-run demo | Beide | ⏳ | 8.6 | Timing + troubleshooting |
| 8.8 | Presentatie deck (optioneel) | Eelco | ⏳ | 8.7 | Slides met key features |
| 8.9 | Go-live & monitoring | Beide | ⏳ | 8.7 | Live testen + alerts checken |

---

## 5. Fasebeschrijving (detail)

### Fase 0 – Setup & Configuratie
**Doel:** Solide basis leggen voor development

**Colin's taken:**
- Initialiseer Next.js project met `create-next-app@latest`
- Configureer TypeScript en basis dependencies
- Neon Postgres database aanmaken (EU-FRA region)
- Prisma initialiseren en database connection testen
- n8n instance opzetten (EU-hosted of cloud)
- Environment variables documenteren in `.env.example`
- OpenAI API key configureren met rate limits

**Eelco's taken:**
- Tailwind CSS configureren en themeing setup
- Installeer shadcn/ui CLI en basis componenten
- Setup Vercel project en link GitHub repository
- ESLint/Prettier configureren
- Documenteer lokale dev setup in README

**Samenwerking:**
- Beide: Git workflow afspreken (feature branches, PR reviews)
- Beide: Standup schema bepalen (bijv. dagelijks 15 min)

---

### Fase 1 – Database & Data Model
**Doel:** Gestructureerde data opslag met test-data

**Colin's taken:**
- Definieer Prisma schema volgens TO (vacancies, candidates, reviews, matches, uploads, audit_logs)
- Maak migraties en test lokaal + productie-strategie
- Schrijf seed script met 2 realistische vacatures (bijv. "Senior Developer", "HR Manager")
- Genereer 5 fictieve kandidaat-profielen met variërende kwaliteit
- Indexeer match_score, created_at voor snelle queries
- Documenteer datamodel in Markdown tabel

**Output:**
```bash
# Colin's deliverables:
prisma/schema.prisma
prisma/migrations/
prisma/seed.ts
docs/datamodel.md
```

---

### Fase 2 – UI Foundation
**Doel:** Professionele interface structuur zonder data

**Eelco's taken:**
- Bouw root layout met header (logo, navigatie) en footer
- Implementeer sidebar navigatie met actieve states
- Integreer shadcn/ui componenten: Button, Card, Table, Dialog, Badge, Tabs
- Maak design tokens (kleuren, spacing, typografie) in Tailwind config
- Bouw skeleton loaders voor alle data-secties
- Test responsive gedrag op 320px, 768px, 1920px
- Voeg error boundary toe met vriendelijke foutmeldingen

**Output:**
```
app/layout.tsx
app/dashboard/page.tsx (leeg)
components/ui/ (shadcn components)
components/layout/Header.tsx
components/layout/Sidebar.tsx
tailwind.config.ts
```

---

### Fase 3 – Backend API & n8n
**Doel:** Werkende API endpoints en n8n workflows

**Colin's taken:**
- **Upload API:** `POST /api/upload` met Vercel Blob storage + Prisma insert
- **Vacancies CRUD:** GET list, GET by id, POST create, PUT update
- **Dashboard API:** Aggregatie queries (avg scores, counts, recent activities)
- **n8n webhook security:** Implementeer HMAC verification met shared secret
- **n8n Flow A:** CV Upload → Text Extraction (pdf-parse) → Store raw_text
- **n8n Flow B:** Trigger Review → OpenAI call → Write to reviews table → Callback
- **n8n Flow C:** Trigger Match → Compare vacancy + cv → Write to matches table → Callback
- **Callback endpoint:** `POST /api/webhooks/n8n` met signature validation
- Error handling: Try/catch in n8n met error-branch naar logging

**API Contract (voorbeeld):**
```typescript
// POST /api/upload
// Body: FormData with file
// Response: { candidate_id: string, status: 'uploaded' }

// GET /api/dashboard
// Response: { 
//   total_candidates: number,
//   total_vacancies: number, 
//   avg_match_score: number,
//   recent_reviews: Review[]
// }
```

**n8n Flow diagram (pseudo):**
```
[Webhook Trigger: /cv-review]
  → Download CV from storage
  → Extract text (pdf-parse node)
  → OpenAI node (Reviewer prompt)
  → Postgres node (INSERT reviews)
  → HTTP Request (callback to app)
  → Error node (on failure → log)
```

---

### Fase 4 – AI Integration
**Doel:** Consistente en gestructureerde AI-output

**Colin's taken:**
- OpenAI client setup met `response_format: { type: "json_object" }`
- **Reviewer Prompt:**
  ```
  System: Je bent een HR-screening assistent. Analyseer cv's objectief.
  User: Analyseer deze kandidaat: [raw_text]
  Output: JSON met summary, strengths[], weaknesses[], scores{}, recommendation
  ```
- **Matcher Prompt:**
  ```
  System: Vergelijk vacature-vereisten met kandidaat-profiel.
  User: Vacature: [description] | Kandidaat: [raw_text]
  Output: JSON met match_score, highlights[], gaps[], actions[]
  ```
- Zod schemas voor runtime validatie van AI-output
- Test met 5 verschillende cv's → consistentie check (variance <10%)
- Log model metadata (model, tokens, latency) in `model_meta` jsonb field
- Temperature: 0.3 (lage variance), max_tokens: 1500

**Deliverables:**
```typescript
// lib/ai/reviewer.ts
export async function reviewCandidate(rawText: string): Promise<ReviewOutput>

// lib/ai/matcher.ts  
export async function matchCandidateToVacancy(
  vacancy: Vacancy, 
  candidate: Candidate
): Promise<MatchOutput>

// lib/ai/schemas.ts (Zod schemas)
```

---

### Fase 5 – Frontend Features
**Doel:** Interactieve UI met echte data

**Eelco's taken:**
- **Dashboard:** Cards met metrics, recent activity lijst, quick actions
- **Vacatures pagina:** Tabel met filters, "Nieuwe vacature" button → modal form
- **Upload interface:** Drag-and-drop zone, file preview, progress indicator
- **Review resultaten:** Score cards (radar chart of bars), samenvatting tekst, sterkte/zwakte lijsten
- **Match dashboard:** Sorteerbare tabel met match_score, highlights badges, "Bekijk profiel" links
- **Kandidaat profiel:** Detail view met tabs (CV, Review, Matches), timeline van activiteiten
- **Filters:** Multi-select voor skills, range slider voor score, search input voor naam
- **Real-time updates:** Polling elke 5s als n8n job bezig is (status indicator)

**UI Components:**
```typescript
// app/dashboard/page.tsx – Metrics + Recent Activity
// app/vacancies/page.tsx – Vacancies Table + Create Modal
// app/vacancies/[id]/page.tsx – Vacancy Detail + Candidates
// app/candidates/page.tsx – Candidates Table + Filters
// app/candidates/[id]/page.tsx – Candidate Profile + Tabs
// components/upload/DropZone.tsx – File upload
// components/review/ScoreCard.tsx – Review visualization
// components/match/MatchTable.tsx – Match results
```

---

### Fase 6 – Rapportage & Export
**Doel:** Professionele PDF-export voor management

**Eelco's taken:**
- Design PDF-template met logo, huisstijl, datum
- Secties: Executive Summary, Top Candidates (top 3), Skills Gap Analysis, Aanbevelingen
- Gebruik `@react-pdf/renderer` voor server-side rendering
- API endpoint: `POST /api/export/pdf` met `vacancy_id` parameter
- Frontend: "Download rapportage" button → fetch PDF blob → download
- (Stretch) Email functionaliteit met Resend/SendGrid

**PDF Structuur:**
```
┌─────────────────────────────┐
│ [Logo]  AI Recruitment Report│
│ Vacature: Senior Developer   │
│ Datum: 17-10-2025            │
├─────────────────────────────┤
│ Executive Summary            │
│ - 12 kandidaten gescreend    │
│ - Avg match: 67%             │
│ - Top skills: React, TS      │
├─────────────────────────────┤
│ Top 3 Kandidaten             │
│ 1. Jan de Vries (89%)        │
│ 2. Sarah Johnson (84%)       │
│ 3. Ahmed Hassan (78%)        │
├─────────────────────────────┤
│ Skills Gap Analyse           │
│ [Chart of missing skills]    │
└─────────────────────────────┘
```

---

### Fase 7 – Integratie & Testing
**Doel:** Alles werkt end-to-end

**Beide developers:**
- Test Flow A: Upload 5 cv's → wacht op n8n callback → bekijk reviews in UI
- Test Flow B: Selecteer vacature → trigger match → zie shortlist in dashboard
- Test Flow C: Klik "Download rapportage" → open PDF → valideer inhoud
- Smoke tests: Alle navigatie links, alle buttons, alle forms
- Error scenarios: Upload zonder bestand, API timeout, invalid JSON van AI
- Performance: Meet tijd voor 10 cv's end-to-end (target: <60s)
- Security: Test webhook zonder signature (moet 401 geven), test rate limits
- Cross-browser: Chrome, Safari, Firefox, Edge

**Testplan document:**
```markdown
# Test Cases
## TC-01: Happy path CV upload
- [ ] Upload 1 PDF → success message
- [ ] n8n callback binnen 10s
- [ ] Review visible in UI

## TC-02: Match flow
- [ ] Select vacancy → select 5 candidates
- [ ] Trigger match → progress indicator
- [ ] Results sorted by score

... etc
```

---

### Fase 8 – Demo-prep & Deploy
**Doel:** Live gaan en demo klaarzetten

**Colin's taken:**
- Zet production environment variables in Vercel
- Run `prisma migrate deploy` op prod database
- Seed prod database met demo-data (2 vacatures, 5 cv's)
- Configureer Sentry (DSN) en PostHog (project key)
- Test prod n8n webhooks → app connectie
- Monitor eerste prod runs in Sentry

**Eelco's taken:**
- Deploy naar Vercel (connect GitHub main branch)
- Configureer custom domain (optioneel)
- Schrijf demo-scenario: 10-min walkthrough met screenshots
- Maak presentatie deck met key features en "wow moments"
- Dry-run demo 2x met timing
- Documenteer troubleshooting tips (wat als X faalt?)

**Demo-scenario (voorbeeld):**
```
1. [0:00] Open dashboard → toon metrics
2. [1:00] Upload 3 cv's → wacht op AI-review
3. [3:00] Toon review resultaten → highlight scores
4. [5:00] Selecteer vacature "Senior Dev" → trigger match
5. [7:00] Bekijk shortlist → sorteer op match_score
6. [8:30] Download PDF rapportage
7. [9:30] Toon PDF → highlight top kandidaten
8. [10:00] Q&A + wrap-up
```

**Go-live checklist:**
- [ ] Vercel deployment green
- [ ] Database connected
- [ ] n8n webhooks reachable
- [ ] Sentry receiving events
- [ ] Demo-data loaded
- [ ] PDF export werkt
- [ ] Performance <60s voor 10 cv's
- [ ] No console errors
- [ ] Mobile responsive checked

---

## 6. Kwaliteit & Testplan

**Testsoorten:**
1. **Unit tests (optioneel):** Zod schema validatie, util functions
2. **Integration tests:** API routes met Prisma mocks
3. **E2E tests:** Playwright voor kritieke flows (upload → review → match)
4. **Smoke tests:** Handmatige checklist voor elke deployment
5. **Performance tests:** Measure n8n flow duration met verschillende cv-groottes

**Acceptatiecriteria:**
- ✅ 5 cv's uploaden en reviewen zonder errors
- ✅ 2 vacatures matchen met consistente scores
- ✅ Dashboard toont realtime metrics
- ✅ PDF export werkt en oogt professioneel
- ✅ Geen console errors of warnings
- ✅ Laadtijd dashboard <2s
- ✅ n8n flow compleet binnen 60s voor 10 cv's

**Tools:**
- Jest + React Testing Library (unit/component tests)
- Playwright (E2E)
- Lighthouse (performance audit)
- Sentry (error monitoring)

---

## 7. Demo & Presentatieplan

**Doel:** In 10 minuten de waarde van AI-recruitment tonen

**Doelgroep:** HR-managers, management, potentiële klanten

**Scenario:**
1. **Intro (1 min):** Probleem → handmatige cv-screening kost tijd
2. **Upload (2 min):** Toon drag-and-drop, real-time processing
3. **Review (3 min):** AI-samenvatting, scores, sterke/zwakke punten
4. **Match (2 min):** Vergelijk met vacature, shortlist genereren
5. **Rapportage (1 min):** Download PDF, toon professional output
6. **Wrap-up (1 min):** Tijdswinst, objectiviteit, schaalbaarheid

**Demo-omgeving:**
- Live op Vercel production URL
- Fallback: Lokale dev server met seed-data
- Backup: Video-opname van volledige flow

**Presentatie-materiaal:**
- Demo-script met timing
- Slides met key benefits (optioneel)
- Fact sheet: "Van 2 uur naar 10 minuten screening"

---

## 8. Risico's & Mitigatie

| Risico | Impact | Waarschijnlijkheid | Verantwoordelijk | Mitigatie |
|--------|---------|--------------------|--------------------|-----------|
| AI-output inconsistent | Hoog | Middel | Colin | Gestructureerde prompts + JSON schema, lage temperature, 5+ test runs |
| n8n webhook timeout | Middel | Laag | Colin | Exponential backoff, error logging, async job queue |
| PDF-generatie faalt | Middel | Laag | Eelco | Fallback: JSON download, test met edge cases |
| Database migratie issues | Hoog | Laag | Colin | Test migrations lokaal, backup prod DB voor deploy |
| Scope creep (te veel features) | Middel | Hoog | Beide | Strikte MVP-scope, "stretch" label voor nice-to-haves |
| Vercel vendor lock-in | Laag | Middel | Beide | Houd codebase portable (standaard Next.js), documenteer alternatieve hosts |
| OpenAI API rate limits | Middel | Laag | Colin | Implementeer retry logic, monitor usage, upgrade tier indien nodig |
| Team blocking elkaar | Middel | Middel | Beide | Duidelijke fase-verantwoordelijkheden, daily standups, feature branches |
| Demo live faalt | Hoog | Laag | Beide | Backup video, lokale fallback, dry-runs (2x) |
| Privacy/AVG issues | Laag | Laag | Beide | Alleen fictieve data in demo, documenteer data handling |

**Mitigatie-acties:**
- Weekly sync: bespreken blockers en dependencies
- Feature flags: nieuwe features behind toggle voor safe rollout
- Monitoring: Sentry alerts op критieke errors
- Documentation: Elke fase heeft deliverables checklist

---

## 9. Taakverdeling & Samenwerking

### Verantwoordelijkheden per Developer

**Colin (Lead - Backend/Infrastructure):**
- ✅ Next.js project initialisatie
- ✅ Database schema en migrations (Prisma)
- ✅ Seed-data en test-data generatie
- ✅ Alle API routes (upload, vacancies, dashboard, webhooks)
- ✅ n8n workflows (parse, review, match)
- ✅ OpenAI integration en prompt engineering
- ✅ Webhook security (HMAC)
- ✅ Sentry en PostHog configuratie
- ✅ Performance monitoring en error handling
- ✅ Production deployment en environment setup

**Eelco (Frontend/UI):**
- ✅ Vercel project setup & hosting
- ✅ Tailwind + shadcn/ui implementatie
- ✅ Alle UI-componenten en layouts
- ✅ Dashboard, vacatures, kandidaten pagina's
- ✅ Upload interface en file handling (client-side)
- ✅ PDF-template design en export-knop
- ✅ Responsive design en cross-browser testing
- ✅ Demo-scenario en presentatie
- ✅ Code quality (ESLint, Prettier)

**Gezamenlijk:**
- ✅ Git workflow en PR reviews
- ✅ Daily standups (15 min)
- ✅ Integratie testing (Fase 7)
- ✅ Demo dry-runs en oplevering
- ✅ Documentation en knowledge sharing

### Communicatie & Workflow

**Daily Standup (15 min):**
- Wat heb ik gisteren gedaan?
- Wat ga ik vandaag doen?
- Zijn er blockers?

**Weekly Sync (30 min):**
- Fase-voortgang review
- Dependency check (wat blokkeert wat?)
- Risico's en mitigaties bespreken
- Demo-voorbereiding

**Git Workflow:**
```
main (protected)
  ├─ colin/feature-database-schema
  ├─ colin/feature-api-routes
  ├─ colin/feature-n8n-flows
  ├─ colin/feature-ai-prompts
  ├─ eelco/feature-ui-foundation
  ├─ eelco/feature-dashboard
  └─ eelco/feature-upload-ui
```

**PR Review Process:**
1. Developer push naar feature branch
2. Create PR met beschrijving en screenshots (indien UI)
3. Andere dev reviewed binnen 24u
4. Merge naar main na approval
5. Vercel auto-deploy naar preview environment

**Dependency Management:**
- Colin's Fase 1 (DB) blokkeert Eelco's Fase 5 (Frontend Features) → Plan buffertijd
- Colin's Fase 3 (API) moet klaar zijn voordat Eelco Fase 5 kan integreren
- Beide werken parallel aan Fase 2 (UI - Eelco) en Fase 1 (DB - Colin) → geen blokkering

**Signalen voor escalatie:**
- Blocker langer dan 1 dag → Direct communiceren
- Fase-delay >2 dagen → Hercalculeren planning
- Kritieke bug in demo-week → Pair programming sessie

---

## 10. Planning & Tijdlijn

**Totale duur:** 3-4 weken (15-20 werkdagen)

| Week | Fases | Colin | Eelco | Milestone |
|------|-------|-------|-------|-----------|
| 1 | Fase 0, 1, 2 | Setup + Database Model | Setup + UI Foundation | ✅ DB + Basis UI klaar |
| 2 | Fase 3, 4 | Backend API + AI Integration | UI componenten afmaken | ✅ API's werkend + AI getest |
| 3 | Fase 5, 6 | n8n troubleshooting + monitoring | Frontend Features + PDF | ✅ Volledige flow werkend |
| 4 | Fase 7, 8 | Testing + Deployment | Testing + Demo-prep | ✅ Live + Demo klaar |

**Kritieke paden:**
- Fase 1 (DB) moet af voordat Fase 3 (API) kan beginnen → Colin prioriteit week 1
- Fase 3 (API) moet af voordat Fase 5 (Frontend Features) kan integreren → Colin week 2
- Fase 7 (Testing) vereist alle vorige fases → Beide week 3-4

**Buffer:**
- 2 dagen reserved voor onvoorziene issues
- 1 dag extra voor demo-prep en troubleshooting

---

## 11. Evaluatie & Lessons Learned

**Na elke sprint (wekelijks):**
- Wat ging goed?
- Wat kan beter?
- Welke blockers hadden we?
- Hoe kunnen we volgende sprint verbeteren?

**Na oplevering (post-mortem):**
- Haalden we de MVP-scope?
- Wat waren de grootste uitdagingen?
- Welke tech choices zouden we anders doen?
- Wat hebben we geleerd over AI-integratie?
- Hoe was de samenwerking (frontend/backend split)?

**Feedback-punten om te loggen:**
- AI-prompt tuning kostte meer/minder tijd dan verwacht
- n8n vs. direct API calls in Next.js → tradeoffs
- Vercel deployment soepel of issues?
- Prisma vs. Drizzle → performance & DX
- shadcn/ui snelheid en customization

**Template voor evaluatie:**
```markdown
## Sprint [X] Retrospective
**Datum:** [dd-mm-jjjj]

### 👍 Wat ging goed
- [item 1]
- [item 2]

### 👎 Wat kan beter
- [item 1]
- [item 2]

### 💡 Learnings
- [item 1]
- [item 2]

### 🎯 Acties voor volgende sprint
- [ ] Actie 1 (owner)
- [ ] Actie 2 (owner)
```

---

## 12. Referenties & Bijlagen

**Projectdocumenten:**
- [PRD – AI Recruitment Assistant](./ai_recruitment_prd.md)
- [FO – Functioneel Ontwerp](./ai_recruitment_fo.md)
- [TO – Technisch Ontwerp](./ai_recruitment_technisch_ontwerp_to_draft.md)
- [Mission Control Template](./mission_control_template.txt)

**Tech Stack Documentatie:**
- [Next.js App Router Docs](https://nextjs.org/docs)
- [Prisma Docs](https://www.prisma.io/docs)
- [shadcn/ui Components](https://ui.shadcn.com)
- [n8n Workflows](https://docs.n8n.io)
- [OpenAI API Reference](https://platform.openai.com/docs)

**Tools & Services:**
- Repository: [GitHub URL]
- Hosting: [Vercel URL]
- Database: [Neon Dashboard URL]
- n8n Instance: [n8n URL]
- Monitoring: [Sentry URL]
- Analytics: [PostHog URL]

**Contacten:**
- Colin Lit – Lead / Backend & Infrastructure – [email]
- Eelco – Frontend & UI – [email]

---

## 13. Appendix: Checklists

### ✅ Definition of Done (per fase)

**Fase = Done wanneer:**
- [ ] Alle subfases hebben status "✅ Gereed"
- [ ] Code is reviewed en gemerged naar main
- [ ] Tests (unit/integration) zijn toegevoegd waar relevant
- [ ] Documentatie is bijgewerkt (README, inline comments)
- [ ] Vercel preview deployment werkt zonder errors
- [ ] Demo-data is getest met de nieuwe functionaliteit
- [ ] Geen blockers voor volgende fase

### ✅ PR Checklist

**Voor elke Pull Request:**
- [ ] Branch naam volgt conventie (`colin/feature-X` of `eelco/feature-Y`)
- [ ] PR beschrijving heeft context en screenshots (indien UI)
- [ ] Code heeft inline comments bij complexe logica
- [ ] Geen hardcoded secrets of API keys
- [ ] ESLint en TypeScript errors opgelost
- [ ] Tested locally in dev-modus
- [ ] Vercel preview link werkt
- [ ] Requested review van andere developer

### ✅ Deploy Checklist

**Voor production deployment:**
- [ ] Alle tests zijn groen
- [ ] Database migrations zijn getest lokaal
- [ ] Environment variables zijn geconfigureerd in Vercel
- [ ] Sentry DSN is geconfigureerd
- [ ] PostHog project key is ingesteld
- [ ] n8n webhooks zijn geconfigureerd voor prod
- [ ] Demo-data is geseed in prod database
- [ ] Vercel deployment is successful (geen warnings)
- [ ] Smoke test uitgevoerd op prod URL
- [ ] Monitoring dashboards zijn opgezet

### ✅ Demo Checklist

**Voor demo-presentatie:**
- [ ] Demo-scenario is geschreven en getest (2x dry-run)
- [ ] Presentatie deck is klaar (optioneel)
- [ ] Vercel prod URL is bereikbaar
- [ ] Fallback: lokale dev server is geconfigureerd
- [ ] Backup: video-opname van volledige flow
- [ ] Demo-data is fris (recent created_at timestamps)
- [ ] PDF export werkt en ziet er professioneel uit
- [ ] Browser tabs zijn voorbereid (geen random tabs)
- [ ] Timing is geoefend (10 min target)
- [ ] Q&A antwoorden zijn voorbereid

---

**Laatste update:** 17-10-2025  
**Status:** Bouwplan goedgekeurd en gereed voor start 🚀  
**Volgende actie:** Fase 0 kickoff meeting + repo setup

