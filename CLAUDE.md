# CLAUDE.md

Vägledning för Claude Code i det här repot. Läs den här filen innan du ändrar något.

## Projekt

Statisk one-page-sajt för **Bahko Byrå** — marknadsföringsbyrå i Stockholm. Tagline: "Synlighet som säljer".

Vanilla HTML, CSS och JavaScript. Ingen byggkedja, inga beroenden, ingen `package.json`. Enda externa resursen är Google Fonts (Cormorant Garamond + Montserrat), laddad från `index.html`.

All text i gränssnittet är på svenska (`<html lang="sv">`). Skriv svensk copy, inte översatt engelska.

## Filkarta

| Fil | Innehåll |
|---|---|
| `index.html` | All markup. Sektionerna i ordning: header/nav → `.hero` → `#tjanster` → `#om` → `#arbete` → `#kontakt` (CTA + formulär) → footer. |
| `styles.css` | Design tokens i `:root` först, sedan komponentblock i samma ordning som markupen (kommentarsavdelare `/* ---------- Namn ---------- */`), responsiva regler sist. |
| `script.js` | En IIFE med `"use strict"`. Gör exakt två saker: togglar mobilmenyn (`.nav-toggle` ↔ `.nav-links.open`, håller `aria-expanded` i synk) och fyller `#year` i footern. |

Lägg nya sektioner på rätt plats i båda filerna — markupordningen och CSS-ordningen ska matcha.

## Kör & granska

```
python3 -m http.server 8000
```

Öppna `http://localhost:8000`. Det finns ingen build, ingen test-svit och ingen linter — granskning sker i webbläsaren. Kontrollera alltid **1440px** (desktop) och **390px** (mobil) innan du säger att något är klart.

## Design tokens

Definierade i `styles.css` `:root`. Använd dem — hårdkoda aldrig ett värde som redan finns som token.

| Token | Värde | Används till |
|---|---|---|
| `--cream` | `#f1ece3` | Sidbakgrund, text på navy |
| `--cream-soft` | `#f7f3ec` | Alternerande sektionsbakgrund |
| `--navy` | `#1b2042` | Rubriker, primärknapp, CTA-sektion |
| `--navy-soft` | `#2a3158` | Gradienter, sekundära ytor |
| `--gold` | `#c5a363` | Accent: eyebrow, kursiva ord, ramar, hover |
| `--gold-light` | `#d8bd87` | Accent på mörk bakgrund (formulärlabels) |
| `--ink` | `#1b2042` | Brödtextfärg (samma som navy) |
| `--muted` | `#6f7186` | Brödtext, labels, sekundär text |
| `--line` | `rgba(27, 32, 66, 0.12)` | Alla tunna avdelare och kortramar |
| `--radius` | `4px` | Hörnradie — sajten är medvetet nästan kantig |
| `--container` | `1140px` | Maxbredd via `.container` |
| `--serif` | Cormorant Garamond | Rubriker, siffror, logotyp |
| `--sans` | Montserrat | Brödtext, nav, knappar, labels |

Behöver du en ny färg eller ett nytt mått: lägg det i `:root` först, använd det sedan.

## Konventioner

- **Rubriker** i `--serif`, `font-weight: 500` eller `600`. Ett `em` inuti en rubrik blir kursivt och guldfärgat (`.hero-title em`, `.section-title em`) — det är så accenten sätts, inte med extra klasser.
- **`.eyebrow`** står ovanför varje sektionsrubrik: versaler, `letter-spacing: 4px`, guld. På mörk bakgrund: `.eyebrow .eyebrow-light`.
- **Brödtext** i `--sans` med `color: var(--muted)`.
- **Knappar**: bara `.btn` plus `.btn-primary` (navy → guld på hover), `.btn-ghost` (guldram) eller `.btn-block` (full bredd). Skapa ingen ny knappvariant utan att fråga.
- **Sektionspadding**: `clamp(70px, 10vw, 130px) 0`. Bakgrunden alternerar `--cream` / `--cream-soft` sektion för sektion — behåll rytmen.
- **Responsivt**: exakt två brytpunkter finns, `860px` och `480px`. Lägg dina regler i de befintliga `@media`-blocken istället för att införa nya brytpunkter.
- **Fluid typografi** med `clamp()` — inte med brytpunkter.
- **Tillgänglighet**: behåll `aria-label` på logotyplänkarna och `aria-expanded` på `.nav-toggle`. Nya interaktiva element ska nås med tangentbord och ha synligt fokus.
- Kort, platt CSS. Inga preprocessorer, inga utility-ramverk, ingen nesting.

## Ton & röst

Kort och konkret svenska. Resultatfokus utan floskler. Du-form mot kunden. Rubriker får vara självsäkra, brödtext ska vara rak. Inga superlativ som inte går att belägga.

## Hårda regler

Det här är inte stilpreferenser — bryt dem inte utan uttrycklig instruktion från Mathias.

1. **Inga priser.** Skriv aldrig ett belopp i kod eller copy om du inte fått exakt siffra av Mathias i samma konversation. Inga "från"-priser, inga paketpriser, inga tidigare prisnivåer återanvända ur minnet.
2. **Inga påhittade kunder, case, omdömen eller mätvärden.** Korten i `#arbete` (Nordström & Co, Lumen Studio, Vera Interiör) och siffrorna i `.hero-stats` (150+, 98%, 12 år) är **platshållare** från första utkastet. Byt inte ut dem mot andra påhittade siffror — flagga dem för Mathias och be om riktiga uppgifter.
3. **Inga default-val.** När flera alternativ är rimliga — namn, layout, färgriktning, formulering — presentera alternativen och låt Mathias välja. Luta inte automatiskt åt ett förvalt alternativ och genomför det inte som om det var bestämt.
4. **Kontaktuppgifter ändras bara på uttrycklig instruktion.** `hej@bahkobyra.se`, telefonnummer och adress i footern rör du inte annars.
5. **Formuläret skickar ingenting.** `#kontakt`-formuläret har `action="#"` och saknar backend. Utlova inte att förfrågningar kommer fram, och bygg ingen inskickningslogik utan att fråga först.
6. **Inga nya beroenden.** Inga ramverk, byggsteg, npm-paket eller CDN-skript utan att fråga först. Sajten ska fortsätta fungera genom att bara öppna `index.html`.

## Skills

Projektets egna skills ligger i `.claude/skills/`. Skriv `/bb` för att se dem alla.

| Skill | När |
|---|---|
| `/bb-sektion` | Lägga till eller bygga om en sektion på sidan |
| `/bb-copy` | Skriva eller revidera svensk copy |
| `/bb-design` | Färg, typografi och tokens |
| `/bb-scroll-cinematic` | Scroll-effekter: reveals, stagger, parallax, sticky |
| `/bb-granska` | Genomgång innan publicering |
| `/bb-forhandsvisa` | Starta sajten och ta skärmbilder |
