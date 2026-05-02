# FastRead — RSVP Reader

Webová aplikace pro rychlé čtení technikou **RSVP (Rapid Serial Visual Presentation)**.
Text se zobrazuje slovo po slovu. Uprostřed každého slova je červeně zvýrazněno písmeno
v bodě přirozeného rozpoznání (**ORP — Optimal Recognition Point**).

## Spuštění

Otevři `index.html` v prohlížeči — žádný server není potřeba.

Pro GitHub Pages: Settings → Pages → Source: main / root

## Funkce

- RSVP přehrávání s ORP zvýrazněním
- Načítání EPUB souborů (parsuje se lokálně, data neopustí zařízení)
- Sleep check: po 15 minutách se zobrazí potvrzovací dialog
- Persistence: kniha i pozice se pamatují (localStorage)
- Historie čtení s datem, časem a procentem
- Navigace: slider, ±slova, ±minuty, přeskočení na pozici z historie
- Rychlost 60–900 WPM, dynamic pacing (interpunkce zpomalí)
- Tmavý/světlý režim (podle systému)
- Test tokenizéru pro ladění problematických textů

## Technologie

Vanilla JS + HTML/CSS. Jediná závislost: **JSZip 3.10.1** (CDN) pro parsování EPUB.

## Roadmapa

1. **Teď** — ladění tokenizéru na reálných EPUB textech, záložky, nastavení fontu
2. **PWA** — manifest.json + service worker → „Přidat na plochu" na iOS i Android
3. **React Native (Expo)** — nativní mobilní appka, iCloud sync pozice
