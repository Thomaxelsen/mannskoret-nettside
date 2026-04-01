# Mannskoret nettside — lokal redesign

## Prosjektoversikt

Lokal redesign av [mannskoret.org](https://mannskoret.org). Utgangspunktet er en kopi av den eksisterende WordPress-siden (Astra-tema), som bygges om side for side til et moderne, minimalistisk design.

- **GitHub:** `Thomaxelsen/mannskoret-nettside` (privat)
- **Branch `legacy`:** Uendret kopi av originalsiden per 2026-03-31
- **Branch `main`:** Redesignet versjon under utvikling

## Teknisk stack

- **Vanilla HTML** — én fil per side, ingen byggsteg
- **Tailwind CSS via CDN** (`<script src="https://cdn.tailwindcss.com">`)
- **Font:** Outfit (Google Fonts CDN)
- **Ikoner:** Phosphor Icons (CDN)
- **Animasjoner:** GSAP + ScrollTrigger (CDN) — kun på forsiden
- **Ingen Node.js, ingen npm, ingen React**

## Designprinsipper

- **Sort/hvitt** — ingen fargeaksenter
- **Minimalistisk** — lite tekst, mye luft, la bildene snakke
- **Ikke AI-slop** — følg `/taste-frontend`-reglene: ingen Inter, ingen lilla, ingen emoji, ingen generiske 3-kort-layouts
- **Konsistent nav og footer** på tvers av alle sider
- **Responsivt** — mobil + desktop

## Filstruktur

```
lokal-dev/
  index.html              — Forside (hero + konserter)
  booking.html            — Booking til events
  medlemskap.html         — Bli med i koret?
  lane-korarrangementer.html — Låne korarrangementer (gammel design)
  historien.html           — Hele historien (gammel design)
  css/                    — Legacy Astra CSS (brukes av uoppdaterte sider)
  js/                     — Legacy Astra JS
  fonts/                  — Legacy Astra fonter
  images/                 — Alle bilder
```

## Lokal utvikling

```bash
cd lokal-dev
python -m http.server 8765
# Åpne http://127.0.0.1:8765/
```

## WordPress-tilkobling

MCP-server `claudeus-wp-mcp` er konfigurert i `~/.claude/.mcp.json` med API-tilgang til mannskoret.org (bruker: ThomasAxelsen). Kan brukes for å pushe innhold tilbake når redesignet er klart.

## Nav-struktur (beholdes på alle sider)

- Booking til events → `booking.html`
- Bli med i koret? → `medlemskap.html`
- Låne korarrangementer → `lane-korarrangementer.html`
- Hele historien → `historien.html`

Logo: `typologo-white.png` (over mørk bg) / `typologo-dark.png` (over lys bg)

## Dokumentasjonskrav

**Ved hver commit skal `STATUS.md` oppdateres** med hva som er endret og nåværende status per side. Dette sikrer at enhver agent eller utvikler som plukker opp arbeidet vet nøyaktig hvor vi står.
