---
name: bb-sektion
description: Lägg till, bygg om eller ta bort en sektion på Bahko Byrå-landningssidan. Använd när användaren vill ha en ny sektion (t.ex. priser, FAQ, team, process, referenser), flytta om sektioner, eller ändra strukturen i index.html. Triggar på "ny sektion", "lägg till en del", "bygg om hero", "flytta tjänsterna", eller när något ska in på sidan som inte finns än.
---

# Ny eller ändrad sektion

Sajten är en one-pager. En sektion är alltid tre saker på tre platser: markup i `index.html`, ett CSS-block i `styles.css`, och eventuellt en nav-länk.

## 1. Placera markupen

Sektionerna i `index.html` ligger i den här ordningen inuti `<main>`:

```
.hero → #tjanster → #om → #arbete → #kontakt
```

Sätt in den nya sektionen där den hör hemma i läsflödet. Mönstret:

```html
<section class="NAMN" id="ANKARE">
    <div class="container">
        <header class="section-head">
            <p class="eyebrow">Kort överrubrik</p>
            <h2 class="section-title">Rubrik med ett <em>guldord</em></h2>
            <p class="section-intro">En rad som sätter sammanhanget.</p>
        </header>
        <!-- innehåll -->
    </div>
</section>
```

- `id` är gemener utan å/ä/ö (`#tjanster`, inte `#tjänster`) — det är befintlig konvention.
- `.section-intro` är valfri. `.eyebrow` och `.section-title` ska alltid med.
- `<em>` inuti rubriken blir kursivt guld automatiskt. Använd det på ett ord, inte flera.
- `.section-head` är centrerad. Vill du vänsterställd rubrik, följ mönstret i `#om` (`.about-text .section-title` sätter `text-align: left`).

## 2. Lägg CSS-blocket på motsvarande plats

`styles.css` följer samma ordning som markupen, med avdelare:

```css
/* ---------- Namn ---------- */
```

Sätt blocket mellan sektionerna det ligger mellan i HTML. Regler:

- Sektionspadding: `padding: clamp(70px, 10vw, 130px) 0;`
- Bakgrunden alternerar. Titta på sektionen före och efter och välj den som håller växlingen `--cream` / `--cream-soft` intakt.
- Bara tokens — `var(--navy)`, `var(--gold)`, `var(--muted)`, `var(--line)`. Aldrig råa hex-värden.
- Kort med ram: kopiera `.service-card`-mönstret (`background: var(--cream)`, `border: 1px solid var(--line)`, hover som lyfter `-6px` med `box-shadow` och guldram).
- Grid: `display: grid` med `gap` i steg om 4px (befintliga är 20/24/28px).

## 3. Gör den responsiv

Det finns **två** brytpunkter, `860px` och `480px`, längst ner i filen. Lägg dina regler i de befintliga blocken — skapa inga nya brytpunkter. Fler-kolumns-grid kollapsar till `grid-template-columns: 1fr` vid 860px.

## 4. Nav-länk

Ska sektionen nås från menyn, lägg till i `.nav-links` i `index.html`:

```html
<li><a href="#ANKARE">Etikett</a></li>
```

`.nav-cta` (Kontakt) ska alltid vara sist i listan. Mobilmenyn kräver ingen JS-ändring — `script.js` stänger menyn vid klick på vilken `<a>` som helst.

## 5. Innan du är klar

- Varje `href="#..."` på sidan matchar ett existerande `id`.
- Sektionen ser rätt ut vid 1440px och 390px (`/bb-forhandsvisa`).
- Ingen påhittad data. Behöver sektionen priser, kundnamn, siffror eller omdömen — **fråga Mathias**, hitta inte på. Se de hårda reglerna i `CLAUDE.md`.
