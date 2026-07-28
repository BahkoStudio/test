---
name: bb-copy
description: Skriv eller revidera svensk copy för Bahko Byrå i varumärkets röst — rubriker, brödtext, eyebrows, knapptexter, meta-description. Använd när användaren vill ändra text på sajten, vill ha förslag på formuleringar, tycker att något "låter platt", eller ber om copy till en ny sektion. Triggar på "skriv om texten", "bättre rubrik", "formulera", "låter tråkigt", "copy".
---

# Copy för Bahko Byrå

Bahko Byrå är en marknadsföringsbyrå i Stockholm. Tagline: **Synlighet som säljer**. All copy på svenska.

## Röst

- **Kort och konkret.** Hellre sju ord än fjorton. Punkt istället för semikolon.
- **Resultat, inte process.** "Synlighet som omsätts i affärer" slår "helhetslösningar inom digital kommunikation".
- **Du-form** mot kunden. "Vi" om byrån.
- **Inga floskler.** Stryk: helhetslösning, skräddarsydd, passion, i framkant, kreativa lösningar, 360-graders, unik.
- **Självsäkert, inte skrikigt.** Rubriker får ta plats. Brödtext ska vara rak.
- Sajten säger själv "byggd på resultat, inte floskler" — copyn måste leva upp till det.

## Format per texttyp

| Typ | Regel |
|---|---|
| `.eyebrow` | 1–3 ord, versaler renderas av CSS — skriv normal skrift ("Vad vi gör", inte "VAD VI GÖR") |
| `.hero-title` | Max ~8 ord, får radbrytas med `<br>`, **ett** ord i `<em>` för guldaccent |
| `.section-title` | 3–7 ord, gärna ett `<em>`-ord |
| `.hero-lead` / `.section-intro` | En mening, max ~25 ord |
| Kortbrödtext | 1–2 meningar, max ~20 ord |
| Knapptext | 2–4 ord, imperativ ("Boka ett möte", "Se våra tjänster") |
| `<title>` | `Bahko Byrå — <fördel>`, under 60 tecken |
| meta description | 1 mening, 120–155 tecken, nämn Bahko Byrå och vad kunden får |

Typografiska detaljer: tankstreck `—` med spatier, `&amp;` i HTML när `&` ska visas, inga utropstecken.

## Spärrar — gäller utan undantag

1. **Inga priser.** Skriv aldrig ett belopp. Inte "från X kr", inte paketpriser, inte "prisvärt från". Behöver texten prissättning: fråga Mathias efter exakta siffror och vänta på svar.
2. **Inga påhittade siffror.** Inga "+X% trafik", "Y nöjda kunder", "Z år i branschen" som du hittat på. De som står på sajten i dag är platshållare — ersätt dem inte med nya påhittade, be om riktiga.
3. **Inga påhittade kundnamn, case eller omdömen.** Kortnamnen i `#arbete` är platshållare, inga riktiga uppdrag.
4. **Inga garantier eller löften** om leveranstid, resultat eller svarstid som inte är bekräftade. Sajten säger "inom 24 timmar" i CTA:n — verifiera med Mathias innan du upprepar eller skärper sådant.
5. **Inga default-förslag.** Ska du ge alternativ — ge faktiska alternativ, tydligt uppställda, och låt Mathias välja. Skriv inte in ditt favoritalternativ direkt i filen och nämn de andra i förbifarten.

## Arbetsgång

1. Läs den befintliga texten runt om — copyn ska sitta ihop med sina grannar.
2. Vid större omskrivningar: visa 2–3 alternativ i chatten först, låt Mathias välja, ändra sedan filen.
3. Vid små justeringar: ändra direkt och citera före/efter.
4. Ändrar du en rubrik med `<em>` — behåll `<em>`-taggen, annars försvinner guldaccenten.
5. Ändra aldrig `hej@bahkobyra.se`, telefonnummer eller adress utan uttrycklig instruktion.
