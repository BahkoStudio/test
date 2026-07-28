---
name: bb-scroll-cinematic
description: Bygg filmiska scroll-effekter på Bahko Byrå-sajten — reveals när sektioner kommer in i vy, staggade kort, parallax, sticky scener, uppräknande siffror, progress-indikator. Använd när användaren vill att sajten ska "leva", kännas premium, animeras vid scroll, eller nämner scroll-animationer, fade-in, parallax, cinematic. Triggar på "scroll-effekt", "animera", "fade in", "parallax", "känns stilla", "mer liv i sajten", "cinematic", "rörelse".
---

# Filmiska scroll-effekter

Bahko Byrå är en stram, elegant sajt — serif-rubriker, guldaccenter, nästan kantiga former. Rörelsen ska matcha det: **återhållsam och tung, inte peppig.** Saker glider in, de hoppar inte. En besökare ska känna kvaliteten utan att kunna peka på animationen.

## Grundregler

1. **Inga bibliotek.** Ingen GSAP, ingen AOS, ingen Framer Motion. `IntersectionObserver` + CSS-transitions räcker för allt nedan. Sajten ska fortsätta fungera utan byggsteg.
2. **Animera bara `transform` och `opacity`.** Aldrig `top`, `height`, `margin` eller `filter` i en scroll-loop — det tvingar layout och hackar.
3. **Innehåll syns utan JS.** Sätt start-tillståndet från JS (eller under en `.js-ready`-klass på `<html>`), så att en besökare med blockerad JS ser färdig sida istället för tom.
4. **`prefers-reduced-motion` respekteras alltid.** Ingen rörelse — men innehållet ska vara fullt synligt.
5. **Reveal sker en gång.** `observer.unobserve(el)` efter att elementet visats. Inget som fadar ut igen när man skrollar upp.
6. **Ingen scroll-jacking.** Rör inte `scroll-behavior: smooth` som redan finns, kapa inte hjulet, hijacka inte scrollhastigheten.

## Rörelsespråk

| Parameter | Värde |
|---|---|
| Distans | `24px` för brödtext/kort, `40px` för stora rubriker. Aldrig mer. |
| Duration | `0.7s` för text, `0.9s` för stora element |
| Easing | `cubic-bezier(0.16, 1, 0.3, 1)` — snabb start, lång utrullning |
| Stagger | `80ms` mellan syskon, max `5` steg innan det känns segt |
| Trigger | När `18–20%` av elementet syns, eller `rootMargin: "0px 0px -12% 0px"` |

Guldaccenter (`.eyebrow`, `<em>` i rubriker) får komma in **strax efter** sin rubrik, inte samtidigt — det är där det filmiska sitter.

## Mönster 1 — Reveal (grunden)

CSS i ett eget block sist i `styles.css`, före `@media`-blocken:

```css
/* ---------- Scroll reveal ---------- */
.reveal {
    opacity: 0;
    transform: translateY(24px);
    transition: opacity 0.7s cubic-bezier(0.16, 1, 0.3, 1),
                transform 0.7s cubic-bezier(0.16, 1, 0.3, 1);
}

.reveal.is-visible {
    opacity: 1;
    transform: none;
}

.reveal-lg {
    transform: translateY(40px);
    transition-duration: 0.9s;
}

@media (prefers-reduced-motion: reduce) {
    .reveal,
    .reveal-lg {
        opacity: 1;
        transform: none;
        transition: none;
    }
}
```

JS inuti den befintliga IIFE:n i `script.js` — lägg till, skapa ingen andra IIFE:

```js
var reduced = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
var targets = document.querySelectorAll(".reveal");

if (reduced || !("IntersectionObserver" in window)) {
    for (var i = 0; i < targets.length; i++) {
        targets[i].classList.add("is-visible");
    }
} else {
    var io = new IntersectionObserver(function (entries) {
        entries.forEach(function (entry) {
            if (!entry.isIntersecting) return;
            entry.target.classList.add("is-visible");
            io.unobserve(entry.target);
        });
    }, { threshold: 0.18, rootMargin: "0px 0px -12% 0px" });

    for (var j = 0; j < targets.length; j++) {
        io.observe(targets[j]);
    }
}
```

Sätt sedan `class="reveal"` på `.section-head`, `.about-text`, `.about-points`, `.cta-inner` och `class="reveal reveal-lg"` på `.hero-title` och `.section-title` där de står ensamma.

Behåll `var` och `"use strict"`-stilen — `script.js` är skrivet så, avvik inte.

## Mönster 2 — Staggade kort

Ge behållaren `.stagger` och låt CSS trappa barnen. Ingen JS-loop med `setTimeout`:

```css
.stagger > * {
    opacity: 0;
    transform: translateY(24px);
    transition: opacity 0.7s cubic-bezier(0.16, 1, 0.3, 1),
                transform 0.7s cubic-bezier(0.16, 1, 0.3, 1);
}

.stagger.is-visible > * { opacity: 1; transform: none; }
.stagger.is-visible > *:nth-child(2) { transition-delay: 80ms; }
.stagger.is-visible > *:nth-child(3) { transition-delay: 160ms; }
.stagger.is-visible > *:nth-child(4) { transition-delay: 240ms; }
```

Passar `.service-grid` (4 kort), `.work-grid` (3 kort), `.hero-stats` (3 siffror), `.about-points`. Lägg `.stagger` **på gridet**, inte på korten. Observera behållaren precis som `.reveal` — samma `is-visible`-klass, så den befintliga observern täcker den om du lägger `.stagger` i selektorn (`".reveal, .stagger"`).

Kom ihåg motion-spärren för `.stagger > *` också.

## Mönster 3 — Parallax i hero

Diskret djup, aldrig mer än ~`60px` total förskjutning. Läs scroll i `requestAnimationFrame`, inte i scroll-handlern:

```js
var hero = document.querySelector(".hero-inner");
var ticking = false;

if (hero && !reduced) {
    window.addEventListener("scroll", function () {
        if (ticking) return;
        ticking = true;
        window.requestAnimationFrame(function () {
            var y = window.pageYOffset;
            if (y < 700) {
                hero.style.transform = "translateY(" + (y * 0.12) + "px)";
                hero.style.opacity = String(1 - y / 900);
            }
            ticking = false;
        });
    }, { passive: true });
}
```

`{ passive: true }` är inte valfritt. Stäng av parallax under `860px` — på mobil kostar det mer än det ger.

## Mönster 4 — Uppräknande siffror

`.hero-stats` innehåller `150+`, `98%`, `12 år`. Vill Mathias ha uppräkning: läs slutvärdet ur `textContent`, parsa siffran, behåll suffixet, räkna upp över `1.2s` med `requestAnimationFrame`, kör **en gång** när `.hero-stats` blir synlig.

Två saker: sätt `min-width` på `.stat-num` så layouten inte hoppar när siffran växer, och kom ihåg att **dessa siffror är platshållare** (se `CLAUDE.md`) — animera dem inte på ett sätt som gör dem mer trovärdiga än de är. Fråga Mathias om riktiga tal först.

## Mönster 5 — Sticky scen

För en sektion som ska "hålla kvar" medan innehållet växlar: `position: sticky; top: 88px` (headerns höjd) på det som ska stå still, i en behållare som är högre. Ren CSS, ingen JS. Använd sparsamt — en sticky scen per sida, annars känns sajten seg.

Headern är redan `sticky` med `z-index: 100`. Nya sticky-element måste ligga under det.

## Mönster 6 — Scroll-progress

En `2px` guldlinje längst upp som fylls med scroll-djupet. Sätt `transform: scaleX()` med `transform-origin: left`, uppdatera i samma `requestAnimationFrame`-loop som parallaxen — skapa ingen andra scroll-lyssnare.

## Innan du är klar

1. Skrolla igenom hela sidan i `/bb-forhandsvisa` — varje sektion ska ha visats. En reveal som aldrig triggar ger tomt innehåll, vilket är värre än ingen animation.
2. Testa `prefers-reduced-motion`: allt innehåll synligt, noll rörelse.
3. Kontrollera 390px — reveals ska fungera, parallax vara av, ingen horisontell scroll från `translate`.
4. Skrolla snabbt och långsamt. Hackar det, är det för att något annat än `transform`/`opacity` animeras.
5. Mobilmenyn ska fungera oförändrat — rör inte den delen av `script.js`.
