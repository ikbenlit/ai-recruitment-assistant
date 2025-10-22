# 🧩 Functioneel Ontwerp (FO) – AI Recruitment Assistant

**Projectnaam:** AI Recruitment Assistant  
**Versie:** v1.0  
**Datum:** 17-10-2025  
**Auteur:** Colin Lit  

---

## 1. Doel en relatie met het PRD
🎯 **Doel van dit document:**  
Het Functioneel Ontwerp beschrijft hoe de AI Recruitment Assistant functioneel werkt: wat HR-medewerkers, recruiters en managers in de interface zien, doen en ervaren. Waar het PRD uitlegt *wat en waarom*, legt dit FO uit *hoe* het prototype zich gedraagt in de praktijk.

📘 **Toelichting:**  
De app laat zien hoe een HR-afdeling AI kan gebruiken om binnenkomende cv’s te beoordelen, te matchen aan vacatures en resultaten te visualiseren in een overzichtelijk dashboard. Het prototype wordt ingezet als demo- en showcaseversie.

---

## 2. Overzicht van de belangrijkste onderdelen
1. **Dashboard / Overzicht** – toont alle lopende vacatures en bijbehorende analyses.  
2. **CV Uploadmodule** – uploaden en parsen van cv’s (PDF of formulier).  
3. **Vacaturebeheer** – toevoegen of selecteren van een vacatureprofiel.  
4. **AI Reviewer** – AI-samenvatting van elk cv met scorekaart.  
5. **AI Matcher** – vergelijking tussen vacature en cv’s met matchscore.  
6. **Rapportage / Export** – downloadbare rapporten voor management.  
7. *(Stretch)* **Bias & Language Check** – detectie van inclusieve of exclusieve taal.

---

## 3. Userstories

| ID | Rol | Doel / Actie | Verwachte waarde | Prioriteit |
|----|------|---------------|------------------|-------------|
| US-01 | Recruiter | Cv’s uploaden via webformulier of PDF | Snelle en consistente invoer van kandidaten | Hoog |
| US-02 | Recruiter | Vacatureprofiel selecteren of invoeren | Context voor AI-analyse | Hoog |
| US-03 | Recruiter | AI-review per cv bekijken | In één oogopslag zien wie relevant is | Hoog |
| US-04 | Recruiter | Matchscore met vacature genereren | Objectieve vergelijking kandidaten | Hoog |
| US-05 | HR-manager | Dashboardoverzicht bekijken | Inzicht in status, scores, en doorlooptijd | Hoog |
| US-06 | HR-manager | Rapportage downloaden | Managementrapport met topkandidaten | Middel |
| US-07 | HR-manager | Gemiddelde matchtrends analyseren | Verbetering wervingsstrategie | Middel |
| US-08 | Demo-user | Snel door flow klikken zonder data-input | Toonbare demo voor klanten | Hoog |

**User Story Template:**  
> Als [rol/gebruiker] wil ik [doel of actie] zodat [reden/waarde].

---

## 4. Functionele werking per onderdeel

### 4.1 Dashboard / Overzicht
* Toont alle actieve vacatures met bijbehorende statistieken: aantal kandidaten, gemiddelde matchscore, top 3 skills.  
* Knoppen: *Nieuwe analyse starten*, *Bekijk rapportage*, *Upload cv’s*.  
* Leeg-staat: melding “Nog geen analyses uitgevoerd”.  

### 4.2 CV Uploadmodule
* Gebruiker kiest één of meerdere cv-bestanden (PDF of formulier).  
* Systeem valideert bestandstype en grootte.  
* n8n start parsing → tekst en metadata worden opgeslagen in database.  
* Na parsing verschijnt bevestiging met overzicht van kandidaten.  

### 4.3 Vacaturebeheer
* Gebruiker kiest bestaande vacature of voert handmatig in (titel, omschrijving, vereisten).  
* Vacature wordt opgeslagen in database als referentie voor matching.  

### 4.4 AI Reviewer
* AI verwerkt elke kandidaat en genereert:  
  - Samenvatting profiel  
  - Sterke/zwakke punten  
  - Score per criterium (Relevantie, Senioriteit, Communicatie, Stabiliteit)  
* Output verschijnt in kaartenweergave met filteropties.  

### 4.5 AI Matcher
* AI vergelijkt vacaturetekst en cv-inhoud.  
* Resultaat: matchscore in %, lijst met ontbrekende skills, aanbevelingen.  
* HR kan filteren op score of naam.  

### 4.6 Rapportage / Export
* Gebruiker kiest *Genereer rapportage*.  
* AI maakt samenvatting van analyses: gemiddelde score, skills-gap, tijdswinst.  
* Export als PDF met huisstijl (logo, kleuren, datum).  

### 4.7 *(Stretch)* Bias & Language Check
* AI controleert cv’s en vacatureteksten op gender-coded of sturende taal.  
* Output: suggesties voor neutralere formulering.  

---

## 5. UI-overzicht (functionele structuur)
```
┌──────────────────────────────────────────────┐
│ Header: logo, gebruiker, instellingen         │
├───────────────┬──────────────────────────────┤
│ Zijbalk       │ Hoofdcontent                 │
│ (Vacatures,   │ Dashboard met tabbladen:     │
│ Kandidaten)   │ - Overzicht                  │
│               │ - AI Reviewer                │
│               │ - Matcher                    │
│               │ - Rapportage                 │
├───────────────┴──────────────────────────────┤
│ Footer: status, versienummer, feedback       │
└──────────────────────────────────────────────┘
```

---

## 6. Interacties met AI (functionele beschrijving)

| Locatie | AI-actie | Trigger | Output |
|----------|-----------|----------|---------|
| CV Upload | CV-analyse (review) | Upload voltooid | JSON met samenvatting + scores |
| Vacature | Context-analyse | Vacature opslaan | Extractie van vereisten en kernwoorden |
| Matcher | Vergelijking cv ↔ vacature | Klik op *Analyseer match* | Matchscore + ontbrekende skills |
| Dashboard | Rapportage genereren | Klik op *Genereer rapportage* | PDF met overzicht van resultaten |
| Bias Check *(Stretch)* | Inclusieve taalcontrole | Klik op *Analyseer taal* | Lijst met risicowoorden + suggesties |

---

## 7. Gebruikersrollen en rechten

| Rol | Toegang tot | Beperkingen |
|------|--------------|-------------|
| Recruiter | Upload, Review, Matcher | Alleen interne demo-data |
| HR-manager | Dashboard, Rapportage | Alleen lezen, geen datawijzigingen |
| Demo-user | Alles | Alleen fictieve data, read-only |

---

## 8. Bijlagen & Referenties
- PRD – AI Recruitment Assistant  
- TO (Technisch Ontwerp) – volgt  
- UX/UI Wireframes – dashboard, upload en rapportage  
- Mission Control Document – bouwfasering  
- AI Prompt Library – Reviewer + Matcher prompts  

