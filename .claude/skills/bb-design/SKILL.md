---
name: bb-design
description: Arbeta med Bahko Byrås designsystem — färger, typografi, spacing, tokens i styles.css. Använd när användaren vill ändra en färg, justera typografi, lägga till en ny token, fixa något som "ser fel ut", eller när du är osäker på vilket värde som ska användas. Triggar på "ändra färgen", "för stort/litet", "spacing", "typsnitt", "kontrast", "designsystem", "ser inte snyggt ut".
---

# Designsystem — Bahko Byrå

Grundregel: **allt värde som finns som token används som token.** Ett rått hex-värde eller ett magiskt px-mått i ett komponentblock är en bugg.

## Tokens

Definierade i `:root` i toppen av `styles.css`:

```
--cream #f1ece3        --navy #1b2042        --gold #c5a363
--cream-soft #f7f3ec   --navy-soft #2a3158   --gold-light #d8bd87
--ink #1b2042          --muted #6f7186       --line rgba(27,32,66,.12)
--radius 4px           --container 1140px
--serif "Cormorant Garamond", Georgia, serif
--sans  "Montserrat", -apple-system, BlinkMacSystemFont, sans-serif
```

Så används de:

- **`--cream`** sidbakgrund och kortytor; även textfärg ovanpå navy.
- **`--cream-soft`** varannan sektion (`.services`, `.work`, `.site-footer`) — växlingen skapar rytmen.
- **`--navy`** rubriker, `.btn-primary`, hela `.cta`-sektionen.
- **`--gold`** accentfärgen: `.eyebrow`, kursiva `<em>` i rubriker, ramar, hover-tillstånd. Använd den sparsamt — den tappar verkan om allt är guld.
- **`--gold-light`** guld på mörk bakgrund (formulärlabels i `.cta`), där `--gold` blir för dov.
- **`--muted`** all sekundär text.
- **`--line`** varje tunn avdelare och kortram, `1px`.

Ny färg eller nytt mått: lägg i `:root` med ett namn som beskriver rollen, inte utseendet (`--line`, inte `--ljusgra`). Ändra sedan **inte** befintliga tokens för att lösa ett lokalt problem — det slår igenom överallt.

## Typografi

- **Serif** (`--serif`) för rubriker, stora siffror (`.stat-num`, `.service-num`) och logotypen. Vikt `500` eller `600`.
- **Sans** (`--sans`) för brödtext, nav, knappar, labels.
- **Fluid storlek med `clamp()`**, inte brytpunkter:
  - `.hero-title` `clamp(44px, 8vw, 86px)`
  - `.section-title` `clamp(34px, 5vw, 52px)`
  - `.cta-title` `clamp(34px, 5vw, 56px)`
  - brödtext `clamp(17px, 2.2vw, 20px)` i hero, annars fast `16–17px`
- **Radhöjd**: rubriker `1.04–1.1`, brödtext `1.6` (satt på `body`).
- **Versaler med letter-spacing** för småtext: `.eyebrow` `4px`, nav `1px`, knappar `1.5px`, labels `1.5–2px`. Aldrig versaler i brödtext.
- Kursiv används bara för accent (`<em>` i rubriker, `.service-num`), aldrig för emfas i brödtext.

## Spacing & layout

- Maxbredd via `.container` (`--container` + `24px` padding). Lägg inte egna maxbredder på sektionsnivå — snäva textblock får `max-width` inuti (`.hero-inner 860px`, `.section-head 640px`, `.cta-inner 720px`).
- Sektionspadding `clamp(70px, 10vw, 130px) 0`.
- Grid-gap: `20px`, `24px` eller `28px`. Inte däremellan.
- `--radius` är `4px` och sajten är medvetet nästan kantig. Runda inte av kort eller knappar.
- Övergångar `0.2–0.35s ease`. Hover som lyfter kort: `translateY(-6px)` + `box-shadow: 0 22px 50px rgba(27,32,66,.1)`.

## Kontrast

Kontrollera alltid nya kombinationer:

- Navy på cream och cream på navy — bra, håll dig där för text.
- **Guld på cream har låg kontrast.** Använd `--gold` för accenter, ramar och stora rubrikord — inte för brödtext eller småtext som måste kunna läsas.
- På navy-bakgrund: använd `--cream` för text och `--gold-light` för accent, aldrig `--gold`.
- Placeholder-text i formuläret är `rgba(241,236,227,.4)` — sänk den inte ytterligare.

## Efter en designändring

Titta på sidan i båda bredderna (`/bb-forhandsvisa`) — `clamp()`-värden och grid-kollapser går inte att bedöma från koden. Ändrar du en token, kontrollera var den används innan du committar:

```
grep -n "var(--TOKEN)" styles.css
```
