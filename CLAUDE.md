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
  lane-korarrangementer.html — Låne korarrangementer
  historien.html           — Hele historien (scroll-basert storytelling)
  kontakt.html            — Kontakt (ny side)
  museum.html             — Grafisk museum (PLANLAGT, ikke startet)
  css/                    — Legacy Astra CSS (brukes av uoppdaterte sider)
  js/                     — Legacy Astra JS
  fonts/                  — Legacy Astra fonter
  images/                 — Bilder brukt på nettstedet
  museum/                 — Plakatarkiv (46 plakater, 2003–2024, sortert i årsmapper)
```

## Lokal utvikling

```bash
cd lokal-dev
python -m http.server 8765
# Åpne http://127.0.0.1:8765/
```

## WordPress-tilkobling

MCP-server `claudeus-wp-mcp` er konfigurert i `~/.claude/.mcp.json` med API-tilgang til mannskoret.org (bruker: ThomasAxelsen, Application Password "Claude Code v2").

### Sider live på WordPress

| Side | WP Page ID | Slug |
|------|-----------|------|
| Forside | 406 | `hjem` |
| Historien | 407 | `historien` |
| Booking | 408 | `booking` |
| Medlemskap | 409 | `medlemskap` |
| Låne korarrangementer | 410 | `lane-korarrangementer` |
| Kontakt | 411 | `kontakt` |

### Deploy-prosedyre (lokal → WordPress)
Scriptet `$TEMP/wp_final_push.py` (må gjenskapes ved ny sesjon) gjør:
1. Ekstraher `<head>` style-blokker + `<body>` innhold fra lokal HTML
2. Erstatt `images/` med WP media-URLer (hardkodet mapping, se STATUS.md)
3. Erstatt `*.html`-lenker med `/slug/`-format
4. Wrap i `<!-- wp:html -->` med Astra CSS override + CDN-ressurser
5. POST til `https://mannskoret.org/wp-json/wp/v2/pages/{id}`

**NB:** WP legger til `-scaled` på store bilder ved upload. Mapping må være hardkodet, ikke automatisk generert fra filnavn.

## Nav-struktur (beholdes på alle sider)

- Booking til events → `booking.html`
- Bli med i koret? → `medlemskap.html`
- Låne korarrangementer → `lane-korarrangementer.html`
- Hele historien → `historien.html`

Logo: `typologo-white.png` (over mørk bg) / `typologo-dark.png` (over lys bg)

## Dokumentasjonskrav

**Ved hver commit skal `STATUS.md` oppdateres** med hva som er endret og nåværende status per side. Dette sikrer at enhver agent eller utvikler som plukker opp arbeidet vet nøyaktig hvor vi står.
