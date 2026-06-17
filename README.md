# Performance onderzoek — sinbad-online.nl

## Testmethoden
Er zijn drie verschillende tools gebruikt om de performance van sinbad-online.nl te meten. Elke tool hanteert een andere methode en geeft daardoor een ander perspectief.

*   **Lighthouse:** Een gesimuleerde laboratoriumtest uitgevoerd vanuit Chrome DevTools. De test simuleert een desktop-omgeving zonder netwerk- of CPU-throttling. De score is een momentopname en kan per run licht variëren.
*   **PageSpeed Insights (Core Web Vitals):** Gebruikt echte gebruikersdata verzameld via Chrome over de afgelopen 28 dagen, op echte mobiele apparaten en verbindingen. Dit is de meest representatieve meting van hoe echte bezoekers de site ervaren.
*   **WebPageTest:** Een laboratoriumtest uitgevoerd vanaf een externe server in Council Bluffs, Iowa (VS) op desktop met Chrome v145, WiFi (240/120 Mbps, 2ms RTT). Door de geografische afstand tot de Nederlandse server zijn sommige waarden hoger dan bij Lighthouse.

---

## Bevindingen per metric

### First Contentful Paint (FCP)
*   **Lighthouse:** 1.9s
*   **PageSpeed:** 2.1s
*   **WebPageTest:** 2.0s
*   *Conclusie:* Consistent beeld across alle drie de tests, allemaal matig (grens is 1.8s).

### Largest Contentful Paint (LCP)
*   **Lighthouse:** 2.3s
*   **PageSpeed:** 2.8s
*   **WebPageTest:** 2.9s
*   *Conclusie:* Lighthouse is het meest optimistisch. PageSpeed en WebPageTest komen dicht bij elkaar en geven een realistischer beeld. Allemaal matig tot slecht.

### Cumulative Layout Shift (CLS)
*   **Lighthouse:** 0.003
*   **PageSpeed:** 0.23
*   **WebPageTest:** 0.009
*   *Conclusie:* Lighthouse en WebPageTest meten groen, maar PageSpeed (echte gebruikers) meet 0.23, bijna rood. Het CLS-probleem is dus alleen zichtbaar onder echte gebruiksomstandigheden op tragere verbindingen.

### Total Blocking Time (TBT)
*   **Lighthouse:** 820ms
*   **WebPageTest:** 277ms
*   *Conclusie:* Het grote verschil komt door de CPU-throttling die Lighthouse toepast. PageSpeed meet dit niet direct.

### Time to First Byte (TTFB)
*   **PageSpeed:** 1.9s (rood)
*   *Conclusie:* Wijst op een serverprobleem dat losstaat van de frontend.

### Interaction to Next Paint (INP)
*   **PageSpeed:** 102ms (groen)
*   *Conclusie:* Interacties reageren snel genoeg ondanks de zware JavaScript-belasting.

---

## Algemene conclusie
De Lighthouse-score op desktop is **45 (rood)**. De site zakt voor de Core Web Vitals evaluatie in PageSpeed doordat LCP en CLS niet in de groene zone zitten. 

### Belangrijkste knelpunten:
*   **Server-vertraging:** De TTFB van 1.9s wijst op een serverprobleem dat alle andere metrics omhoog trekt. Zelfs met een geoptimaliseerde frontend begint elke pageload al met een achterstand.
*   **JavaScript-belasting:** Dit is de zwaarste bottleneck aan de frontend-kant:
    *   15.4s main-thread work
    *   5.3s execution time
    *   181 KiB ongebruikte JavaScript

## Licentie

This project is licensed under the terms of the [MIT license](./LICENSE).
