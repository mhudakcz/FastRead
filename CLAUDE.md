# CLAUDE.md — instrukce pro práci na FastRead

## Repozitář
https://github.com/mhudakcz/FastRead

## Co je projekt
FastRead = RSVP Reader. Webová appka pro rychlé čtení (slovo po slovu, ORP zvýraznění).
Cíl: mobilní appka iOS + Android. Aktuální fáze: standalone HTML demo.

## Soubory
- `index.html` — kompletní aplikace, vše v jednom souboru
- `README.md` — popis projektu
- `CLAUDE.md` — tento soubor

## Co funguje
- RSVP přehrávání (ORP, dynamic pacing, 60–900 WPM)
- Načítání EPUB (JSZip, lokálně v prohlížeči)
- Sleep check po 15 min (dialog s odpočtem)
- Persistence pozice (localStorage)
- Historie čtení (datum, čas, procento, akce)
- Navigace (slider, ±slova, ±minuty)
- Tokenizér v2 (uvozovky, apostrofy, pomlčky)
- Test tokenizéru (debug panel)
- Dark mode (systémový)

## Co je potřeba dodělat
- [ ] Tokenizér — stále testovat s reálnými EPUB texty
- [ ] Záložky — uložit pozici s poznámkou
- [ ] Nastavení — velikost fontu, font family
- [ ] iOS UX — schovat drag&drop (nefunguje), zvětšit touch targets
- [ ] PWA — manifest.json + service worker
- [ ] React Native (Expo) — nativní appka (fáze 3)

## Tokenizér — logika

```
text
→ normalize newlines
→ em/en pomlčka mezi písmeny → odsadit mezerami
→ 4× průchod: uzavírací interpunkce → otevírací/písmeno = vložit mezeru
   uzavírací: . ! ? , ; : ) ] » " " ' ' — –
   otevírací: „ " ' « ( [ písmeno číslice
→ apostrof po VELKÉM písmenu nebo číslici → rozdělit
→ apostrof před VELKÝM písmenem → rozdělit
→ apostrof na začátku tokenu před malým → rozdělit
→ apostrof mezi malými písmeny → ZACHOVAT (don't, it's, d'artagnan)
→ split na whitespace → pole tokenů
```

Problémové případy k testování (záložka "Test tokenizéru"):
- `slovo."„Přímá řeč"` — chybějící mezera
- `Domin's` / `d'Artagnan` — kontrakce a francouzská jména
- `řekl.'Jdi!'` — apostrof jako otevírací uvozovka
- `slovo—slovo` — em pomlčka bez mezer

## EPUB na iPhone
- `<input type="file">` funguje od iOS 13, otevře Files app
- Drag & drop na iOS **nefunguje** — drop zone schovat/označit
- Zdroje: iCloud Drive, „Na mém iPhonu", Dropbox, Google Drive

## Dynamic pacing (prodlevy)
| Situace | Násobek |
|---|---|
| normální slovo | 1× |
| slovo > 9 znaků | 1.3× |
| slovo končí `,;:` | 1.7× |
| slovo končí `.!?` | 3× |

Základ = 60 000 ms / WPM
