---
name: bb-granska
description: Gå igenom Bahko Byrå-sajten före publicering — länkar, ankare, tillgänglighet, responsivitet, SEO-meta och platshållarinnehåll. Använd när användaren vill kontrollera att sajten är i ordning, ska publicera, frågar "är allt klart?", eller efter en större ändring. Triggar på "granska", "kolla igenom", "innan vi publicerar", "är det något som är trasigt", "checklista".
---

# Granskning före publicering

Gå igenom listan i ordning. Rapportera **allt** du hittar — även det du redan fixat — och avsluta med en tydlig rad om sajten är klar eller inte.

## 1. Länkar och ankare

- Varje `href="#..."` i `index.html` matchar ett existerande `id`. Kontrollera:
  ```
  grep -o 'href="#[^"]*"' index.html | sort -u
  grep -o 'id="[^"]*"' index.html | sort -u
  ```
  Kända ankare: `#top`, `#tjanster`, `#om`, `#arbete`, `#kontakt`.
- `href="#"` utan mål finns i footern (Instagram, LinkedIn) — det är medvetna platshållare. Flagga dem, byt inte ut mot gissade URL:er.
- `mailto:` och `tel:` i footern pekar på rätt uppgifter och är oförändrade.

## 2. Tillgänglighet

- `.nav-toggle` har `aria-label` och `aria-expanded` som växlar `true`/`false` vid klick — verifiera i webbläsaren, inte bara i koden.
- Båda logotyplänkarna har `aria-label`.
- Rubriknivåerna hoppar inte: ett `<h1>` (hero), `<h2>` per sektion, `<h3>` i kort, `<h4>` i footerkolumner.
- Alla formulärfält har en `<label>` med `for` som matchar fältets `id` (`name`, `email`, `message`).
- Tangentbord: tabba genom sidan, alla länkar/knappar/fält nås och har synligt fokus.
- Kontrast: ingen brödtext i `--gold` på ljus bakgrund.

## 3. Responsivt

Kör `/bb-forhandsvisa` och kontrollera:

- **1440px** — grid ligger rätt (tjänster 2 kolumner, arbete 3 kolumner, om 1.4fr/1fr), inget som spiller utanför `.container`.
- **390px** — hamburgaren syns, `.nav-links` glider in vid klick och stängs vid klick på en länk, alla grid kollapsat till en kolumn, formuläret enkolumnigt, ingen horisontell scroll.
- Inget textblock som blir orimligt smalt eller brett däremellan.

## 4. SEO och head

- `<html lang="sv">`.
- `<title>` satt, under 60 tecken.
- `<meta name="description">` satt, 120–155 tecken.
- `<meta name="viewport">` finns.
- Google Fonts-länkarna med `preconnect` är intakta — utan dem faller typografin tillbaka på Georgia/system-sans.

## 5. Innehåll — viktigast

- `#year` i footern fylls faktiskt med årtalet (kontrollera i webbläsaren att det inte står tomt).
- **Platshållare får inte presenteras som fakta.** Dessa är platshållare i dag:
  - `.hero-stats`: `150+`, `98%`, `12 år`
  - `#arbete`-kort: Nordström & Co, Lumen Studio, Vera Interiör med `+210% trafik`, `3x leads`, `+85% försäljning`
  - footerns sociala länkar (`href="#"`)
  - telefon `+46 8 123 456 78` och adress `Storgatan 1, 111 51 Stockholm`

  Lista dem i din rapport och fråga Mathias om riktiga uppgifter. **Hitta aldrig på ersättningar.**
- Inga priser någonstans i markupen om de inte uttryckligen bekräftats:
  ```
  grep -niE 'kr|sek|:-|pris' index.html
  ```
- Ingen kvarglömd `lorem`, `TODO`, `FIXME` eller engelsk platshållartext.

## 6. Formuläret

`#kontakt`-formuläret har `action="#"` och ingen backend — **det skickar ingenting**. CTA:n utlovar svar inom 24 timmar. Påminn Mathias om att formuläret behöver kopplas innan sajten går live, annars försvinner varje förfrågan.

## Rapportformat

```
Klart: <vad som är i ordning>
Fixat: <vad du ändrade>
Kräver beslut: <platshållare och frågor till Mathias>
Blockerar publicering: <ja/nej + varför>
```
