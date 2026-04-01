# Status — Mannskoret nettside redesign

Sist oppdatert: 2026-04-01

## Sidestatus

| Side | Fil | Status | Beskrivelse |
|------|-----|--------|-------------|
| Forside | `index.html` | Ferdig | Hero med konsertbilde + parallax, sticky innholdsseksjon med 3 konserter, footer |
| Booking | `booking.html` | Ferdig | Split-layout: tekst venstre, ansikter-bilde høyre (sort bakgrunn) |
| Bli med i koret | `medlemskap.html` | Ferdig | Split-layout: tekst venstre, stjerne-bilde høyre (scale-trick for sømløs kant) |
| Låne korarrangementer | `lane-korarrangementer.html` | Ferdig | Split-layout: tekst venstre, sort logopanel høyre, watermark-logoer (4 varianter) |
| Kontakt | `kontakt.html` | Ferdig | Tre infobokser: booking, generelle henvendelser, postadresse |
| Hele historien | `historien.html` | Under arbeid | Scroll-basert storytelling — grunnstruktur ferdig, gjenstår: bilder/layout for innledende kapitler, finjustering |

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

## Endringslogg

### 2026-04-01
- Redesignet `historien.html` — scroll-basert storytelling (under arbeid):
  - 15 kapitler med fade-in-animasjoner (IntersectionObserver)
  - Sticky media: Krogh (bilde av Ivar), Zappa (YouTube brandless)
  - Fullskjerm Berlin café-video med tekst over (komprimert 270MB→4MB MP4)
  - Sticky YouTube-trailer for «For vi er gutta» med tekst → video → tekst scrollmønster
  - TV-aksjonen YouTube-klipp inline i Mange oppdrag
  - Husleiekonsert med tre illustrasjonsbilder ved siden av tekst
  - Jimmis Rekordbok med 2016-plakat, Jan-Tore bilde, 1. mai 2013-bilde
  - Lydknapp (mute/unmute) på alle videoer
  - YouTube IFrame API med brandless embed (gradient overlay-maskering)
  - Zappa-video med fade-out ved loop
  - Responsivt: sticky → stacker på mobil
  - WP-vennlig: klasse-baserte selektorer, data-attributter for video-IDer
  - **Gjenstår:** Bilder og layout for innledende kapitler, finjustering av tekst/bilde-balanse
- Redesignet `medlemskap.html` med split-layout og stjerne-bilde
- Fikset bildekant-problem med scale-trick (Gemini)
- Oppdatert nummerte steg-design (Gemini)
- Redesignet `lane-korarrangementer.html` med split-layout, watermark-logoer, og sort logopanel
- Lagt til logo-varianter for watermarks: gunther og 2008-outline (dark/white PNG)
- Opprettet `kontakt.html` med tre infobokser (booking, generelt, postadresse)
- Oppdatert footer på alle sider: Kontakt-lenke peker nå til kontakt.html
- Fikset mobilmeny: byttet bg-black/98 til bg-zinc-950 (solid) på alle sider
- Satt repo til public, lagt til GitHub Pages deploy via Actions workflow
- Live: https://thomaxelsen.github.io/mannskoret-nettside/
- Lagt til Kontakt-lenke i desktop-nav og burger-meny på alle sider

### 2026-03-31
- Opprettet GitHub-repo `Thomaxelsen/mannskoret-nettside` (privat)
- Lastet ned lokal kopi av mannskoret.org → `legacy` branch
- Redesignet `index.html` med hero, parallax, 3 konserter
- Redesignet `booking.html` med split-layout og ansikter-bilde
- Laget transparente PNG-logoer (typologo-white, typologo-dark)
- Konvertert konsertbilder til sort/hvitt
