# Tailwind CSS v3 vs v4 - Configuratie Gids

## Probleem

Tailwind CSS v4 heeft een **compleet nieuwe configuratie-aanpak** die niet compatibel is met v3-configuraties. Als je Tailwind v4 installeert maar een v3-stijl `tailwind.config.js` gebruikt, werkt Tailwind niet correct.

## Symptomen

- Tailwind classes worden niet toegepast
- Styling werkt niet in de browser
- Geen build errors, maar geen visuele output
- `tailwind.config.js` bestaat maar wordt genegeerd

## Oorzaak

**Tailwind v4** gebruikt:
- CSS-first configuratie in plaats van JavaScript
- `@tailwindcss/postcss` plugin
- Configuratie via CSS variabelen in `globals.css`
- Geen `tailwind.config.js` nodig

**Tailwind v3** gebruikt:
- JavaScript configuratie in `tailwind.config.js`
- Standaard PostCSS setup met `autoprefixer`
- Theme extensies via JS-objecten

## Aanbeveling voor Next.js 15 (2025)

**Gebruik Tailwind CSS v3.4.x** omdat:
- ✅ Stabiele, production-ready versie
- ✅ Volledige Next.js 15 support
- ✅ Grote community en documentatie
- ✅ Alle features die je nodig hebt
- ❌ Tailwind v4 is nog in alpha/beta
- ❌ Tailwind v4 heeft breaking changes
- ❌ Tailwind v4 heeft minder documentatie

## Oplossing: Downgrade van v4 naar v3

### Stap 1: Update `package.json`

**Verwijder v4 packages:**
```json
"devDependencies": {
  "@tailwindcss/postcss": "^4",  // ❌ Verwijderen
  "tailwindcss": "^4",           // ❌ Verwijderen
}
```

**Voeg v3 packages toe:**
```json
"devDependencies": {
  "autoprefixer": "^10.4.20",    // ✅ Toevoegen
  "postcss": "^8.4.49",          // ✅ Toevoegen
  "tailwindcss": "^3.4.17",      // ✅ Toevoegen
}
```

### Stap 2: Update `postcss.config.mjs`

**v3 configuratie:**
```javascript
/** @type {import('postcss-load-config').Config} */
const config = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}

export default config
```

### Stap 3: Zorg dat `tailwind.config.js` bestaat

**v3 configuratie (standaard):**
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './src/pages/**/*.{js,ts,jsx,tsx,mdx}',
    './src/components/**/*.{js,ts,jsx,tsx,mdx}',
    './src/app/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {
      colors: {
        // Custom colors
      },
    },
  },
  plugins: [],
}
```

### Stap 4: `globals.css` blijft hetzelfde

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  /* CSS variables voor custom kleuren */
}
```

### Stap 5: Installeer dependencies

```bash
npm install
```

### Stap 6: Herstart dev server

```bash
# Kill oude servers
pkill -f "next dev"

# Start nieuwe server
npm run dev
```

## Verificatie

Na downgrade zou je moeten zien:
- ✅ Tailwind classes werken in de browser
- ✅ Custom kleuren uit `tailwind.config.js` beschikbaar
- ✅ Hot reload werkt voor CSS changes
- ✅ Geen console errors over Tailwind

## Quick Check Commando's

```bash
# Check Tailwind versie
npm list tailwindcss

# Verwachte output voor v3:
# tailwindcss@3.4.17

# Check of PostCSS plugins correct zijn
cat postcss.config.mjs

# Verwachte output:
# plugins: { tailwindcss: {}, autoprefixer: {} }
```

## Wanneer WEL v4 gebruiken?

Overweeg Tailwind v4 als:
- Je een nieuw greenfield project start
- Je wilt experimenteren met nieuwe features
- Je bereid bent om breaking changes te handelen
- Je de nieuwe CSS-first configuratie wilt testen

**Let op:** v4 vereist andere config structuur en migratie-inspanning.

## Resources

- [Tailwind CSS v3 Docs](https://tailwindcss.com/docs/installation)
- [Tailwind CSS v4 Alpha Docs](https://tailwindcss.com/blog/tailwindcss-v4-alpha)
- [Next.js + Tailwind Setup](https://nextjs.org/docs/app/building-your-application/styling/tailwind-css)

## Changelog

- **2025-10-03**: Document aangemaakt na downgrade van v4 → v3 in Agapia MVP
