# Status — Mannskoret nettside redesign

Sist oppdatert: 2026-04-02

## Prosjektstatus: LIVE PÅ WORDPRESS

**Nettsiden er migrert til WordPress og er live på [mannskoret.org](https://www.mannskoret.org/).**

Alle 6 sider er deployet som WordPress-sider med Astra-temaet. Designet serveres som rå HTML via `<!-- wp:html -->`-blokker med Tailwind CDN, GSAP og egne fonter injisert direkte i sideinnholdet. Astra-temaets header/footer er deaktivert per side via meta-felter.

De lokale HTML-filene i dette repoet er kildefilene. Ved endringer: oppdater lokal fil → re-push til WordPress via REST API.

## Live-sider

| Side | Lokal fil | WordPress URL | WP Page ID |
|------|-----------|---------------|------------|
| Forside | `index.html` | [mannskoret.org/](https://www.mannskoret.org/) | 406 (slug: `hjem`) |
| Historien | `historien.html` | [mannskoret.org/historien/](https://www.mannskoret.org/historien/) | 407 |
| Booking | `booking.html` | [mannskoret.org/booking/](https://www.mannskoret.org/booking/) | 408 |
| Medlemskap | `medlemskap.html` | [mannskoret.org/medlemskap/](https://www.mannskoret.org/medlemskap/) | 409 |
| Låne korarrangementer | `lane-korarrangementer.html` | [mannskoret.org/lane-korarrangementer/](https://www.mannskoret.org/lane-korarrangementer/) | 410 |
| Kontakt | `kontakt.html` | [mannskoret.org/kontakt/](https://www.mannskoret.org/kontakt/) | 411 |

## WordPress-oppsett

### Tema og plugins
- **Tema:** Astra v4.4.0 (header/footer deaktivert per side)
- **Plugins:** Kun Akismet (spam)
- **Forsideinnstilling:** Statisk side → ID 406 (`hjem`)

### API-tilgang
- **Bruker:** ThomasAxelsen (Administrator)
- **Autentisering:** Application Password "Claude Code v2" (Basic Auth via REST API)
- **Endepunkt:** `https://mannskoret.org/wp-json/wp/v2/pages/{id}`
- **Konfig:** `~/.claudeus-mcp/wp-sites.json` (oppdatert med nytt passord)

### Hvordan innhold pushes til WordPress
Hver side deployes som én `<!-- wp:html -->`-blokk som inneholder:
1. CSS som skjuler Astra-temaets egne elementer (`display:none!important`)
2. CDN-ressurser (Tailwind, Outfit font, Phosphor Icons, evt. GSAP)
3. Page-spesifikke `<style>`-blokker fra original `<head>` (nav-scroll, mobilmeny-animasjoner osv.)
4. Full `<body>`-innhold (nav + hovedinnhold + footer + inline scripts)
5. Bildestier erstattet med WordPress media-URLer
6. Interne lenker konvertert fra `fil.html` til `/slug/`

### Astra meta-felter (satt per side)
```
ast-main-header-display: disabled
ast-hfb-above-header-display: disabled
ast-hfb-below-header-display: disabled
ast-hfb-mobile-header-display: disabled
footer-sml-layout: disabled
site-sidebar-layout: no-sidebar
ast-site-content-layout: full-width
site-post-title: disabled
site-content-layout: plain-container
```

### Backup av gamle sider
De 5 opprinnelige WordPress-sidene er bevart som draft med `-backup`-suffix:
- ID 144: `144-2-backup` (gammel forside)
- ID 278: `historien-backup`
- ID 216: `medlemskap-backup`
- ID 201: `booking-backup`
- ID 351: `lane-korarrangementer-backup`

## Mediafiler på WordPress

33 filer lastet opp til WP mediebibliotek (ID 373–405):
- 32 bilder (JPG, PNG) — inkl. logoer, ikoner, konsertbilder, historiske bilder
- 1 video: `berlin-cafe-web.mp4` (4.4 MB)
- **NB:** `Ivar.avif` ble konvertert til `Ivar.jpg` fordi WP ikke støtter AVIF

## Sidestatus (design)

| Side | Fil | Beskrivelse |
|------|-----|-------------|
| Forside | `index.html` | Hero med konsertbilde + GSAP parallax, sticky innholdsseksjon med 3 konserter, footer |
| Booking | `booking.html` | Split-layout: tekst venstre, ansikter-bilde høyre (sort bakgrunn) |
| Bli med i koret | `medlemskap.html` | Split-layout: tekst venstre, stjerne-bilde høyre (scale-trick for sømløs kant) |
| Låne korarrangementer | `lane-korarrangementer.html` | Split-layout: tekst venstre, sort logopanel høyre, watermark-logoer (4 varianter) |
| Kontakt | `kontakt.html` | Tre infobokser: booking, generelle henvendelser, postadresse |
| Hele historien | `historien.html` | Scroll-basert storytelling — 15 kapitler, fullscreen seksjoner, sticky video, glassmorphism |
| Grafisk museum | `museum.html` | Opplevelsesbasert arkiv med GSAP-parallax, skråstilte plakater og Lightbox |

## Grafisk museum

**Status:** Fullført lokalt (`museum.html`). Klar for gjennomgang og WP-deploy ved behov.

Opprettet en "Scrollytelling"-side basert på 44 bilde-plakater, organisert i årsmapper. Inverterer sort/hvitt-konseptet ved å vise fargeplakater mot lys bakgrunn, lagt opp med asymmetrisk "punk/paste-up" grid. Store typografiske vannmerker (årstall) driver den vertikale navigasjonen, bygget med GSAP Parallax, komplett med JS Lightbox for bildevisning.


### Plakatarkiv (`museum/`)
46 plakater lastet ned fra Google Drive (MARKEDSFØRING → GRAFISK), organisert i årsmapper:

```
museum/
  2003/  (1 plakat)    2010/  (2 plakater)   2016/  (4 plakater)   2022/  (4 plakater)
  2004/  (1)           2011/  (3)             2017/  (3)             2023/  (3)
  2005/  (1)           2012/  (4)             2018/  (3)             2024/  (2)
  2006/  (2)           2013/  (2)             2019/  (2)
  2007/  (1)           2014/  (3)             2020/  (1)
  2008/  (2)           2015/  (2)
```

Filnavn-konvensjon: `{YYYY-MM-DD}_{event-name}.{ext}` (lowercase, bindestreker).

**Kilde:** Google Drive delt mappe `1w6Cx6D8TZ_CndQMUaadTXaNiy1dY5KzO` → MARKEDSFØRING → GRAFISK.

**Merknader:**
- 2 filer var PDF (2019-05 Trondheim, 2024-02 Hvit Måned) — konvertert til JPG via PyMuPDF
- 1 fil er GIF (2014-06 Husleia)
- Mangler 2009, 2021 (ingen events/plakater funnet)
- Ikke lastet ned: logo, korbilde, t-skjorter, backdrops, stillingsannonser, Raga singel-cover

**Ikke push til WordPress ennå** — jobbes med lokalt og i GitHub først.

## Konserter på forsiden

| Konsert | Dato | Sted | Billett-lenke |
|---------|------|------|---------------|
| Vi Bygger Landet (1. mai) | 1. mai 2026 | Vålerenga kirke | ticketco.events |
| Piknik i Parken | 11. juni 2026 | Sofienbergparken | ticketmaster.no |
| Raga Rockers / Mannskoret | 15. august 2026 | Foynhagen, Tønsberg | ticketmaster.no |

## Designbeslutninger

- **Sort/hvitt** gjennomgående — alle bilder konverteres til grayscale
- **Typologo** (tekstlogo) brukes i nav på alle sider, ikke ikonsymbolet
- **Cava-inspirert scroll-effekt** på forsiden: hero med parallax, innhold glir opp over
- **Split-layout** for undersider med bilder (booking, medlemskap)
- **Bilder med innebygd hvit kant** håndteres med `scale-[1.06]` + `overflow-hidden`

## Teknisk stack

- **Vanilla HTML** — én fil per side, ingen byggsteg
- **Tailwind CSS via CDN** (`<script src="https://cdn.tailwindcss.com">`)
- **Font:** Outfit (Google Fonts CDN, vekter 300–900)
- **Ikoner:** Phosphor Icons (`<script src="https://unpkg.com/@phosphor-icons/web">`)
- **Animasjoner:** GSAP v3.12.5 + ScrollTrigger (CDN) — kun på forsiden
- **Video:** YouTube IFrame API (historien) + lokal MP4 (Berlin café)
- **Ingen Node.js, ingen npm, ingen React**

## Endringslogg

### 2026-04-02
- **MIGRERING TIL WORDPRESS FULLFØRT**
  - Lastet opp 33 mediafiler (32 bilder + 1 video) til WP mediebibliotek
  - Opprettet 6 nye sider med full HTML-innhold som `<!-- wp:html -->`-blokker
  - Deaktiverte Astra header/footer per side via meta-felter
  - Satt ny forside (ID 406) som statisk forside
  - Konverterte bilde-stier til WP media-URLer og interne lenker til WP slugs
  - Injiserte Tailwind CDN, Outfit font, Phosphor Icons og GSAP i sideinnhold
  - Inkluderte `<head>`-style-blokker (nav-scroll, mobilmeny-animasjoner) i body for WP-kompatibilitet
  - Satte opp Application Password v2 for REST API-tilgang
  - Backup av 5 eksisterende WP-sider som draft
  - Verifisert alle sider desktop (1280px) og mobil (375px) med Playwright screenshots
- **WP-spesifikke fikser etter deploy:**
  - Fikset bildmapping: filer med mellomrom/store bokstaver → WP `-scaled` og bindestrek-konvertering
  - Lagt til `color:#fff !important` og `font-weight:700 !important` på fullscreen-overskrifter i historien (Astra CSS overstyring)
  - Mørkere overlay i historien: `bg-black/70` → `/80`, video `/50` → `/75`, pluss `text-shadow`
  - Burgermeny-ikon: lagt til `border border-zinc-300 rounded-md` på alle undersider
  - Forsiden: burgermeny bytter til mørk farge + ramme ved scroll forbi hero
- **Sticky nav** på alle undersider (`sticky top-0 z-50 bg-[#fafafa]/95 backdrop-blur-md`)
- Rettet postadresse på kontaktsiden til Arendalsgata 45, 0463 Oslo
- **Grafisk museum — plakater lastet ned:**
  - 46 plakater hentet fra Google Drive (MARKEDSFØRING → GRAFISK) via MCP
  - Organisert i `museum/` med årsmapper 2003–2024
  - Filnavn standardisert til `{YYYY-MM-DD}_{event-name}.{ext}`
- Oppdatert bilde for Raga Rockers på forsiden (`index.html`) til turné-plakat (`foyn-bw.jpg`)
- Implementert premium 3:4 container med `object-contain` slik at plakaten skalerer pent på mobil uten å strekkes
- Byttet intro-bilde i `historien.html` fra `Sparkle.jpeg` til `2004-05 v2.jpg` (gruppeportrett, sort/hvitt med grayscale)
- **Opprettet `museum.html`:** Implementert Grafisk museum ("Scrollytelling") basert på de 44 asymmetrisk plasserte plakatene fra perioden 2003–2024. Omfatter bl.a. vannmerket typografi for år, asymmetriske plakater animert med Parallax (GSAP ScrollTrigger), rotasjoner, dynamisk uttrekk av hendelser/dato fra filnavn, og skreddersydd skjult Lightbox-skall for fullskjermsvisning av gamle detaljerike plakater.
- **Plakater 2025–2026 lagt til:** Hvit Måned (feb 2025) og Hvit Månedsavslutning (jan 2026) lagt til i grafisk museum. Totalt 46 plakater (2003–2026).
- **PDF-plakater konvertert:** 2019-05 Trondheim og 2024-02 Hvit Måned konvertert fra PDF til JPG (PyMuPDF, 3x zoom). Hvit Måned 2024 senere fjernet (ikke museumsverdig).
- **Museumsdesign V2 (Dark Mode & Konsertatmosfære):**
  - Omskrevet siden til Dark Mode (`#09090b` bakgrunn, hvit/grå navigasjon og typografi).
  - Fjernet bildebeskjæring; plakatveggen viser nå samtlige proporsjoner 100 % for en ujevn og organisk "paste-up"-fasade.
  - Implementert avansert, fotorealistisk konsertmiljø for WordPress-deploy: Fast bakgrunnsbilde (`stage spotlight.jpg`), to flanke-matriser med rullerende røykteppe (`smoke-bg.png`) og gradient-maskeringer mot sentrum.
  - Satt opp kaotisk "swirling & pulsering" for røyken ved å la bildet panorere gjennom et usynlig, modifiserende SVG-filter (`feTurbulence` og `feDisplacementMap`) matet med GSAP/native `hueRotate`-animasjoner.

### 2026-04-01
- Fullført design-oppgradering av `historien.html`:
  - Gjorde om `Hvit Månedsavslutning` og `Flinke Folk` til massive fullscreen lerrets-kapitler med sorte/hvite bilder som bakgrunn og invertert (hvit) cover-tekst.
  - Omformet trange bildekolonner til asymmetriske gallerier (naturlig bildeforhold på plakatene i `Husleiekonsert`, stagger layout i `1. mai` oppslag).
  - Brukte noteark for Jokke og Zappa som kreative vannmerker lagt bak en "glassmorphism" tekstboks (`backdrop-blur`) i Repertoar-kapittelet.
  - La til filene `Sparkle.jpeg` (panorama) og `Stiftelsesprotokoll` (fysisk papir design), fjernet utvalgte placeholdere og byttet bunnbildet med `Mannskoret ansikter.jpg`.
- Redesignet `historien.html` — scroll-basert storytelling:
  - 15 kapitler med fade-in-animasjoner (IntersectionObserver)
  - Sticky media: Krogh (bilde av Ivar), Zappa (YouTube brandless)
  - Fullskjerm Berlin café-video med tekst over (komprimert 270MB→4MB MP4)
  - Sticky YouTube-trailer for «For vi er gutta» med tekst → video → tekst scrollmønster
  - TV-aksjonen YouTube-klipp inline i Mange oppdrag
  - Husleiekonsert med tre illustrasjonsbilder ved siden av tekst
  - Jimmis Rekordbok med 2016-plakat, Jan-Tore bilde, 1. mai 2013-bilde
  - Lydknapp (mute/unmute) på alle videoer
  - YouTube IFrame API med brandless embed (gradient overlay-maskering)
  - Responsivt: sticky → stacker på mobil
- Redesignet `medlemskap.html` med split-layout og stjerne-bilde
- Fikset bildekant-problem med scale-trick
- Redesignet `lane-korarrangementer.html` med split-layout, watermark-logoer, og sort logopanel
- Lagt til logo-varianter for watermarks: gunther og 2008-outline (dark/white PNG)
- Opprettet `kontakt.html` med tre infobokser (booking, generelt, postadresse)
- Oppdatert footer og nav på alle sider med Kontakt-lenke
- Satt repo til public, lagt til GitHub Pages deploy via Actions workflow

### 2026-03-31
- Opprettet GitHub-repo `Thomaxelsen/mannskoret-nettside` (privat)
- Lastet ned lokal kopi av mannskoret.org → `legacy` branch
- Redesignet `index.html` med hero, parallax, 3 konserter
- Redesignet `booking.html` med split-layout og ansikter-bilde
- Laget transparente PNG-logoer (typologo-white, typologo-dark)
- Konvertert konsertbilder til sort/hvitt
- Installert WordPress MCP (`claudeus-wp-mcp`) for API-tilgang
