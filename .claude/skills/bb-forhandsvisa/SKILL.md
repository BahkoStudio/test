---
name: bb-forhandsvisa
description: Starta Bahko Byrå-sajten lokalt och ta skärmbilder i desktop- och mobilbredd med den förinstallerade Chromium. Använd när användaren vill se sajten, vill ha en skärmbild, ber dig kontrollera hur en ändring ser ut, eller när du själv behöver verifiera layout och responsivitet. Triggar på "visa sajten", "hur ser det ut", "skärmbild", "kör sajten", "starta servern", "kolla mobilen".
---

# Förhandsvisa sajten

Sajten är statisk — ingen build. Servera mappen och rendera med Chromium.

## 1. Starta servern

```
python3 -m http.server 8000
```

Kör den i bakgrunden (`run_in_background: true`). Sidan ligger på `http://localhost:8000`.

Serva via HTTP, öppna inte `file://` — Google Fonts och relativa sökvägar beter sig annorlunda där.

## 2. Ta skärmbilder

Chromium är förinstallerad. `PLAYWRIGHT_BROWSERS_PATH=/opt/pw-browsers` är redan satt och `PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=1` stoppar nedladdning. **Kör aldrig `playwright install`.** Hittar Playwright inte browsern, peka på den explicit med `executablePath: '/opt/pw-browsers/chromium'`.

Skriv skriptet till scratchpad-katalogen, inte till repot. Två bredder:

- **desktop** 1440×900
- **mobil** 390×844

```js
const { chromium } = require('playwright');
(async () => {
  const browser = await chromium.launch();
  for (const [name, w, h] of [['desktop', 1440, 900], ['mobil', 390, 844]]) {
    const page = await browser.newPage({ viewport: { width: w, height: h } });
    await page.goto('http://localhost:8000', { waitUntil: 'networkidle' });
    await page.screenshot({ path: `${name}.png`, fullPage: true });
    await page.close();
  }
  await browser.close();
})();
```

`waitUntil: 'networkidle'` behövs — utan det hinner Google Fonts inte in och skärmbilden visar Georgia/system-sans istället för Cormorant/Montserrat.

## 3. Testa mobilmenyn

Vid 390px är hamburgaren synlig. Verifiera i samma körning:

```js
await page.click('.nav-toggle');
// .nav-links ska ha klassen "open", aria-expanded ska vara "true"
await page.click('.nav-links a');
// menyn ska stänga igen, aria-expanded tillbaka till "false"
```

Läs av `aria-expanded` och klasslistan och rapportera vad du faktiskt fick — det är den enda interaktiva funktionen på sajten och den går inte att bedöma från en stillbild.

## 4. Visa resultatet

Skicka skärmbilderna till Mathias med `SendUserFile` (`display: "render"`), en rad om vad som syns, och nämn uttryckligen om något ser fel ut i någon av bredderna.

## 5. Städa

Stoppa bakgrundsservern när du är klar. Lämna inga `.png`-filer eller skript i repot — allt tillfälligt hör till scratchpad-katalogen.
