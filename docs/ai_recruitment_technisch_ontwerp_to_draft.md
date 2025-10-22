# ⚙️ Technisch Ontwerp (TO) – AI Recruitment Assistant (Draft)

**Projectnaam:** AI Recruitment Assistant  
**Versie:** v0.9 (draft)  
**Datum:** 17-10-2025  
**Auteur:** Colin Lit  

> Dit TO sluit aan op het PRD en FO en focust op een **demo/MVP** die op Vercel draait met Next.js. We kiezen pragmatisch voor snelle oplevering, lage kosten en EU‑dataresidentie waar mogelijk.

---

## 1) Scope & aannames
- **Hosting:** Vercel (prod + preview)
- **Framework:** Next.js (App Router) + TypeScript + Tailwind + shadcn/ui
- **Workflow-automation:** n8n (EU), via webhook calls ↔️ app
- **Doel:** HR‑showcase: CV‑review + Match met Vacature, dashboard + PDF export
- **Data:** Fictieve demo‑data (geen echte persoonsgegevens) tijdens MVP

---

## 2) Architectuur-overzicht
```
[Browser]
   │  (upload CV, selecteer vacature)
   ▼
[Next.js (Vercel)]  ──>  /api/upload  ──>  [Storage]
   │                       /api/analyze ──>  [n8n Webhook EU]
   │                               ▲            │  (parse, LLM, JSON)
   │                               │            ▼
   │                           /api/webhooks <─── n8n (callbacks)
   │
   └──> DB (Postgres) <─────────────── Prisma/Drizzle ORM

Observability: Sentry (logs/trace) • PostHog (events) • Vercel Analytics
Rate limits: Edge middleware + Upstash Redis (optioneel)
```

---

## 3) Stack & beslissingen
- **UI:** Next.js 15 (App Router), React Server Components, TypeScript, Tailwind, shadcn/ui
- **ORM:** Prisma (stabiel, snel op te zetten). *Alternatief:* Drizzle
- **DB-keuze:**
  - **Neon Postgres (EU-FRA)** – serverless, goedkoop, Vercel‑vriendelijk
  - *Alternatief:* **Supabase (EU-West)** met RLS + Storage
  - *Alternatief:* **Vercel Postgres** (region afhankelijk)
  - **Keuze MVP:** **Neon Postgres** (EU) + **Supabase (EU-West)** met RLS + Storage voor file storage (PDF's)
- **Auth:** Voor demo: *in-app demo mode* (no auth) of simpele Basic Auth via env. Post‑MVP: NextAuth/Clerk
- **AI‑provider:**
  - **Default:** OpenAI API (structured JSON via response_format)  
  - **EU‑alternatief:** Azure OpenAI (EU region) of Vertex AI (EU) voor enterprise trajecten
- **n8n:** Zelf gehost (EU) of n8n Cloud (EU datacenter). Ingesteld met **signed webhooks** + secret header
- **PDF export:** @react-pdf/renderer of server-side Puppeteer/Playwright (Vercel Function / Edge‑compatible lib)
- **File parsing:**
  - **PDF → text:** pdf-parse / tika via n8n, of in‑app met `pdf-parse`
  - **Virus scan (optioneel):** n8n + ClamAV container step
- **Observability:** Sentry (server + browser), Vercel logs, PostHog voor event‑analytics

---

## 4) Data‑model (MVP)
**Tabel: vacancies**
- id (uuid)
- title (text)
- description (text)
- requirements (jsonb[]) – genormaliseerd als lijst strings
- created_at (timestamptz)

**Tabel: candidates**
- id (uuid)
- full_name (text)
- email (text, null voor demo)
- source (text) – upload/form/demo
- raw_text (text) – uit pdf of formulier
- cv_url (text) – signed URL naar storage (optioneel)
- created_at (timestamptz)

**Tabel: reviews**
- id (uuid)
- candidate_id (fk)
- summary (text)
- strengths (jsonb)
- weaknesses (jsonb)
- scores (jsonb) – {relevance, seniority, communication, stability}
- recommendation (text) – "invite" | "maybe" | "reject"
- model_meta (jsonb)
- created_at (timestamptz)

**Tabel: matches**
- id (uuid)
- vacancy_id (fk)
- candidate_id (fk)
- match_score (numeric)
- gaps (jsonb) – ontbrekende skills/ervaring
- highlights (jsonb) – overeenkomende punten
- actions (jsonb) – suggesties/next steps
- model_meta (jsonb)
- created_at (timestamptz)

**Tabel: uploads** (optioneel)
- id (uuid)
- candidate_id (fk)
- file_name (text), mime (text), size (int)
- storage_key (text), signed_until (timestamptz)
- created_at (timestamptz)

**Tabel: audit_logs**
- id (uuid)
- actor (text) – demo‑user
- event (text) – create_vacancy / upload_cv / run_match / export_report
- payload (jsonb)
- created_at (timestamptz)

> Indexen: `matches(vacancy_id, match_score DESC)`, `reviews(candidate_id)`, `candidates(created_at)`

---

## 5) API‑oppervlak (Next.js routes)
- `POST /api/upload` – upload CV; returns candidate_id
- `POST /api/parse` – (optioneel) parse local; meestal via n8n
- `POST /api/review` – start AI‑review (enqueues job of direct call)
- `POST /api/match` – start match voor (vacancy_id, candidate_ids[])
- `POST /api/vacancies` – create vacancy
- `GET /api/vacancies/:id` – details + topline metrics
- `GET /api/dashboard` – aggregated metrics (avg score, counts)
- `POST /api/export/pdf` – genereer managementrapport
- `POST /api/webhooks/n8n` – ontvangt callbacks (HMAC‑verified header)

**Headers & security**
- `X-Webhook-Secret` (env) tussen app ↔ n8n
- Rate‑limit per IP (middleware) – Upstash Redis (optioneel)

---

## 6) n8n‑flows (functioneel/technisch)
**Flow A: CV → Parse → Review**
1. **Webhook (signed)** ontvangt `candidate_id` + `cv_url`
2. Download file → parse text → normaliseer (tokens, lines)
3. AI Call (Reviewer prompt + JSON schema)
4. Write `reviews` record (pg node) + callback naar `/api/webhooks/n8n`

**Flow B: Vacancy+CVs → Match**
1. **Webhook (signed)** met `vacancy_id` + `candidate_ids[]`
2. Fetch vacancy + candidate.raw_text
3. AI Call (Matcher prompt + JSON schema)
4. Upsert `matches`; callback → `/api/webhooks/n8n`

**Flow C (optioneel):** Bias/Language Check (batch)

> Monitoring in n8n: Error‑branch naar Slack/Discord webhook met run‑id en payload sample.

---

## 7) AI‑prompting & JSON‑schema (kern)
**Reviewer JSON (schets):**
```json
{
  "summary": "string",
  "strengths": ["string"],
  "weaknesses": ["string"],
  "scores": {
    "relevance": 0-10,
    "seniority": 0-10,
    "communication": 0-10,
    "stability": 0-10
  },
  "recommendation": "invite|maybe|reject"
}
```

**Matcher JSON (schets):**
```json
{
  "match_score": 0-100,
  "highlights": ["string"],
  "gaps": ["string"],
  "actions": ["string"],
  "extracted_requirements": ["string"]
}
```

**Prompt‑richtlijnen:**
- Gebruik *system prompt* met rol “HR screening assistant”
- Vraag altijd **geldige JSON** (no prose) – `response_format: { type: "json_object" }`
- Voeg max‑tokens en temperatuur (lage var.) toe voor consistentie
- Logging: sla `model_meta` (model, tokens, latency) op per call

---

## 8) Privacy & security (MVP)
- **Demo‑data only** (geen PII in prod tot DPA geregeld is)
- **Dataresidentie EU:** Neon EU, n8n EU, object storage EU
- **Transport:** altijd TLS (https)
- **Secrets:** Vercel env vars + Vercel Secret Store
- **Webhooks:** HMAC of shared secret; deny by default op IP‑range (optioneel)
- **Sanitization:** strip PII bij demo‑uploads (n8n step)
- **Retention:** truncate tables via script na demo; cron cleanup

---

## 9) Deployment & config
**Vercel Project Settings**
- Framework: Next.js • Build Command: `pnpm build` • Output: default
- Env vars: zie §10
- Cron (optioneel): nightly cleanup → `/api/cron/cleanup`
- Regions: EU (FRA/AMS) waar mogelijk

**Pipelines**
- `main` → Production; PRs → Preview
- DB migrations via Prisma `prisma migrate deploy` in build/postinstall

---

## 10) Environment variables (voorbeeld)
```
# Database
DATABASE_URL=postgres://... (Neon EU)

# Storage
SUPABASE_URL=...
SUPABASE_ANON_KEY=...
SUPABASE_SERVICE_ROLE=...
VERCEL_BLOB_READ_WRITE_TOKEN=...

# AI
OPENAI_API_KEY=...
AZURE_OPENAI_ENDPOINT=...
AZURE_OPENAI_KEY=...

# n8n
N8N_WEBHOOK_URL=https://.../webhook/...
WEBHOOK_SECRET=supersecret

# Observability
SENTRY_DSN=...
POSTHOG_KEY=...
POSTHOG_HOST=...

# App
APP_BASE_URL=https://...
BASIC_AUTH_USER=demo
BASIC_AUTH_PASS=demo
```

---

## 11) Performance & kosten (indicatief MVP)
- **Vercel**: €0–20/maand (hobby → pro)  
- **Neon Postgres**: starter tier ~ gratis/laag  
- **Supabase Storage of Vercel Blob**: paar euro/maand bij lage volumes  
- **OpenAI**: afhankelijk van volume; demo: < €20/maand  
- **n8n**: self‑host goedkoop; cloud EU ~ €20–50/maand

**Optimalisaties:**
- Batch‑calls in n8n (reduce overhead)
- Cache vacature‑embeddings (indien gebruikt)
- Edge‑middleware voor rate limit (misbruik)

---

## 12) Roadmap technische uitbreidingen
- AuthN/AuthZ (NextAuth/Clerk) + roles
- Embeddings + vectordb (Qdrant/PGVector) voor snellere semantische match
- Background jobs (Inngest / Vercel Queues / Upstash Q)
- Fine‑tuned prompts per sector (zorg, overheid, IT)
- Full audit trail + immutable logs (for compliance)

---

## 13) Testplan (kort)
- **Happy paths:** upload → review → match → export (5 cv’s)
- **Edge cases:** lege pdf, extreem lange CV, vreemde encoding
- **Contract:** geldige JSON uit AI (schema‑validate), webhook HMAC check
- **Performance:** 10 CV’s < 60s totale flow (batch via n8n)

---

### ✅ Samenvatting
Met **Vercel + Next.js + Neon + n8n (EU)** hebben we een snelle, goedkope en geloofwaardige setup voor de showcase. We borgen **structured AI‑output**, **EU‑dataresidentie**, en een **duidelijke upgrade‑pad** richting enterprise (auth, bias‑check, ATS‑integraties).

