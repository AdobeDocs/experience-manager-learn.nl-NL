---
title: Real User Monitoring (RUM)
description: Meer informatie over Real User Monitoring (RUM) vindt u op AEM as a Cloud Service website.
version: Cloud Service
feature: Operations
topic: Performance
role: Admin, Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 0
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-03-18T00:00:00Z
source-git-commit: 7c80bb25b79a77c4a0bb2bbedf8a7c338177b857
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---


# Real User Monitoring (RUM)

Meer informatie over Real User Monitoring (RUM) vindt u op AEM as a Cloud Service website. Begrijp hoe te om RUM toe te laten, welke gegevens worden verzameld en hoe te om gegevens RUM te gebruiken om de gebruikerservaring op uw website te optimaliseren.

## Overzicht

Real User Monitoring (RUM) is een methode die wordt gebruikt om _gebruikersinteracties en ervaringen verzamelen, meten en analyseren_ met een website in real time. Het biedt inzicht in de manier waarop sitebezoekers met uw website communiceren, inclusief hun gedrag, prestaties en algemene ervaring. Dit wordt bereikt door een klein stukje JavaScript-code op de pagina&#39;s van de site in te voeren.

Met behulp van JavaScript-code legt RUM gegevens rechtstreeks vast vanuit de browser van de gebruiker wanneer deze op uw website reageren. Deze gegevens kunnen worden gebruikt om prestatieproblemen vast te stellen en te diagnosticeren, gebruikerservaring te optimaliseren en bedrijfsresultaten te verbeteren.

De functie RUM in AEM as a Cloud Service biedt een uitgebreide weergave van de gebruikerservaring van uw website. Hierin worden de volgende maatstaven vastgelegd voor elke pagina (URL) die de gebruiker bezoekt:

- [Grootste inhoudelijke verf (LCP)](https://web.dev/articles/lcp) - de laadprestaties worden gemeten.
- [Cumulatieve layoutverschuiving (CLS)](https://web.dev/articles/cls) - metingen van de visuele stabiliteit.
- [Interactie met volgende verf (INP)](https://web.dev/articles/inp) - de interactiviteit wordt gemeten.
- Weergaven - meet het aantal keer dat een pagina wordt weergegeven.

Ook worden de 404 fouten en voorvertoningsgrafieken voor de website vastgelegd.

De metriek LCP, CLS, en INP maken deel uit van [Kernwebvariaties](https://web.dev/articles/vitals) Dit zijn een reeks meetgegevens die betrekking hebben op snelheid, reactiesnelheid en visuele stabiliteit van een website. Deze meetgegevens worden door Google gebruikt om de gebruikerservaring op een website te meten en zijn belangrijk voor het bepalen van de positie van zoekmachines.

## RUM inschakelen

Als u RUM wilt inschakelen voor uw AEMCS-website, raadpleegt u [Hoe te opstelling de Echte Dienst van de Controle van de Gebruiker](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#how-to-set-up-the-rum-service).

De belangrijkste details van RUM in AEMCS zijn:

- Het RUM is alleen van toepassing op de publicatieservice van AEMCS. Dit houdt in dat JavaScript-code alleen wordt geïnjecteerd in de publicatieomgeving.
- De `com.adobe.granite.webvitals.WebVitalsConfig` OSGi configuratiecontrole omvat en sluit wegen uit, zijn dit bewaarnemerwegen en niet de wegen URL.
- Standaard `/content` pad is opgenomen.
- Als u paden wilt uitsluiten, voegt u de `AEM_WEBVITALS_EXCLUDE` Omgevingsvariabele van Cloud Manager, zie [Omgevingsvariabelen toevoegen](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#add-variables). De paden worden gescheiden door een komma.
- OOTB-code is verantwoordelijk voor het injecteren van de JavaScript-code op de pagina&#39;s.

### Verificatie

Als u wilt controleren of RUM is ingeschakeld voor uw website, bekijkt u de HTML-bron van de gepubliceerde pagina en zoekt u naar de volgende scriptblokken:

```html
...

<!-- Added before the closing </head> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('top');
    window.addEventListener('load', () => sampleRUM('load'));
    document.addEventListener('click', () => sampleRUM('click'));
</script>

...

<!-- Added before the closing </body> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('lazy');
    sampleRUM('cwv');
</script>
```

## RUM-gegevensverzameling

- De RUM-gegevens worden verzameld met `sampleRUM()` door de naam van het controlepunt door te geven. In het bovenstaande voorbeeld zijn de controlepunten `top`, `load`, `click`, `lazy`, en `cwv`.
- Een controlepunt is een benoemde gebeurtenis in de volgorde waarin de pagina wordt geladen en er interactie mee plaatsvindt.

Zie ook [Real User Monitoring Service en Privacy](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#rum-service-and-privacy) en [Welke gegevens worden verzameld](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#what-data-is-being-collected) voor meer informatie .

## RUM-gegevensweergave

Om de RUM gegevens te bekijken u nodig hebt `domainkey`, verstrekt de Adobe het als deel van de opstelling RUM. Het RUM-dashboard is beschikbaar op [https://data.aem.live/](https://data.aem.live/) en u kunt tot het toegang hebben gebruikend de domeinsleutel en url.

Onder screenshot ziet u bijvoorbeeld het RUM-dashboard voor de AEM WKND-website.

![RUM-dashboard](./assets/rum/RUM-Dashboard-WKND.png)

Het RUM-dashboard biedt de volgende belangrijke inzichten:

- **Prestatiewaarden** - LCP, CLS, INP, en Paviews.
- **Foutstatistieken** - 404 fouten.
- **Diagrammen** - Aantal pagina&#39;s in de loop van de tijd.

## Hoe worden RUM-gegevens gebruikt

Met bovenstaande inzichten kunt u de gebruikerservaring op uw website optimaliseren. Bijvoorbeeld:

- Verminder LCP, CLS, en INP om de prestaties en de interactiviteit van het paginading te verbeteren. Zie [Hoe te om LCP te verbeteren](https://web.dev/articles/lcp#improve-lcp), [CLS verbeteren](https://web.dev/articles/cls#improve-cls) en [Hoe te om INP te verbeteren](https://web.dev/articles/inp#improve-inp)voor meer informatie .
- Om de gebruikerservaring te verbeteren, bevestig de 404 fouten.
- Analyseer de diagrammen om het gebruikersgedrag te begrijpen en de inhoud te optimaliseren.

Adobe beveelt aan het RUM-dashboard regelmatig te herzien, met name na een grote of kleine release.

Zie ook [Wie kan profiteren van de Real User Monitoring Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#who-can-benefit-from-rum-service) voor meer informatie .
