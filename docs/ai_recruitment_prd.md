# 📄 Product Requirements Document (PRD) – AI Recruitment Assistant

**Projectnaam:** AI Recruitment Assistant  
**Versie:** v1.0  
**Datum:** 17-10-2025  
**Auteur:** Colin Lit  

---

## 1. Doelstelling

🎯 **Doel:**  
Een werkend prototype ontwikkelen dat laat zien hoe AI de **selectie- en screeningsfase van HR** kan versnellen, objectiveren en standaardiseren.  

📘 **Toelichting:**  
De app toont hoe een HR-afdeling met AI en n8n een eerste selectie van cv’s kan automatiseren. Door AI-samenvattingen, matchscores en visuele dashboards wordt het proces sneller, consistenter en beter onderbouwd.  

Het prototype dient als **showcase** voor management en klanten: een concreet bewijs van hoe AI direct toegevoegde waarde biedt in operationele HR-processen.  

---

## 2. Doelgroep

🎯 **Doel:** HR-afdelingen en management overtuigen van de praktische waarde van AI.  

📘 **Primaire doelgroepen:**  
- **HR-managers / Recruiters** → willen sneller geschikte kandidaten selecteren.  
- **Teamleiders / Management** → willen transparante, datagedreven rapportages over de kwaliteit van instroom.  
- **IT / Innovatieafdelingen** → zoeken veilige, schaalbare AI-toepassingen zonder black-box risico.  

**Belangrijkste behoefte:**  
→ Minder tijd besteden aan handmatige cv-screening, meer consistentie en beter inzicht in de kwaliteit van kandidaten.  

---

## 3. Kernfunctionaliteiten (MVP-scope)

1. **CV Upload & Parsing**  
   - HR uploadt 1 of meerdere cv’s (PDF of formulier).  
   - n8n parseert tekst en slaat structuur op in database (Postgres / Supabase).  

2. **Vacature Import / Profielselectie**  
   - HR selecteert een bestaande vacaturetekst of voert een nieuwe in.  

3. **AI CV Reviewer**  
   - AI genereert per kandidaat een rapport met:  
     - Samenvatting van profiel  
     - Sterke en zwakke punten  
     - Scores (Relevantie, Senioriteit, Communicatie, Stabiliteit)  

4. **AI Match met Vacature**  
   - AI berekent matchscore (%) en vergelijkt vereisten vs. skills.  
   - Genereert “AI shortlist” van topkandidaten.  

5. **Dashboard (HR-view)**  
   - Overzicht per vacature met:  
     - Kandidatenlijst + matchscores  
     - Filters (score, skills, ervaring)  
     - AI-samenvatting per persoon  

6. *(Stretch)* **Rapport Export / PDF**  
   - Genereert managementrapportage met shortlist, metrics en aanbevelingen.  

---

## 4. Gebruikersflows (Demo- of MVP-flows)

### Flow 1 – CV-analyse & Review
1. HR uploadt 5 cv’s in webapp.  
2. n8n start parsing → database-entry per kandidaat.  
3. AI-reviewer maakt rapporten (JSON-output → UI).  
4. Recruiter bekijkt resultaten → ziet scores & samenvatting per kandidaat.  

### Flow 2 – Match met Vacature
1. HR kiest een vacature uit lijst.  
2. n8n vergelijkt vacature met alle cv’s.  
3. AI berekent matchscores + ontbrekende competenties.  
4. Dashboard toont shortlist + AI-advies.  

### Flow 3 – Rapportage & Management Review
1. Recruiter klikt op “Genereer rapportage”.  
2. AI maakt samenvatting van de instroom:  
   - Gemiddelde matchscore  
   - Top ontbrekende vaardigheden  
   - Tijdswinst t.o.v. handmatige screening  
3. PDF wordt automatisch gemaild of gedownload.  

---

## 5. Niet in Scope

> - Rollenbeheer / accountsysteem  
> - Integratie met bestaande ATS-systemen  
> - Productie-grade beveiliging of real-user data  
> - Feedback-loop training op echte HR-data  

---

## 6. Succescriteria

- Prototype werkt volledig in een live-demo (≤ 10 minuten).  
- AI genereert consistente reviews zonder fouten in output.  
- Dashboard toont duidelijke shortlist met scores.  
- Minimaal 1 demo-vacature + 5 test-cv’s beschikbaar.  
- Rapportage export functioneert en oogt professioneel.  

---

## 7. Risico’s & Mitigatie

| Risico | Impact | Mitigatie |
|--------|---------|-----------|
| AI-output inconsistent | Hoog | Gebruik gestandaardiseerde promptstructuur met JSON schema |
| CV-parsing fouten | Middel | Beperk tot testformaten; handmatige controle in demo |
| Scope creep (te veel features) | Middel | Alleen Reviewer + Matcher in MVP |
| Privacy / AVG | Laag | Gebruik fictieve testdata, geen persoonsgegevens |
| Performance bij veel bestanden | Laag | Limiteer uploads tot 10 cv’s per run |

---

## 8. Roadmap / Vervolg (Post-MVP)

- Integratie met ATS (Recruitee, AFAS, etc.)  
- Bias Detector module (gendered language / inclusiviteit)  
- Culture Fit analyse op basis van bedrijfswaarden  
- Recruiter-feedbackloop voor modelverbetering  
- Real-time dashboard met HR-analytics (Power BI / Supabase Charts)  
- Multi-role user access & API-integratie  

---

## 9. Bijlagen & Referenties

- Functioneel Ontwerp (FO) – volgt na goedkeuring PRD  
- Technisch Ontwerp (TO) – n8n + AI promptstructuur  
- UX/UI Wireframes – dashboard + rapportage  
- Mission Control Document – fasering & bouwplan  
- AI Prompt Library – reviewer + matcher prompts  

