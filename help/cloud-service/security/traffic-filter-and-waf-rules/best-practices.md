---
title: Aanbevolen werkwijzen voor verkeersfilterregels, inclusief WAF-regels
description: Leer geadviseerde beste praktijken voor het vormen van de regels van de verkeersfilter met inbegrip van de regels van WAF in AEM as a Cloud Service om veiligheid te verbeteren en risico's te verlichten.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 0%

---

# Aanbevolen werkwijzen voor verkeersfilterregels, inclusief WAF-regels

Leer geadviseerde beste praktijken voor het vormen van de regels van de verkeersfilter met inbegrip van de regels van WAF in AEM as a Cloud Service om veiligheid te verbeteren en risico&#39;s te verlichten.

>[!IMPORTANT]
>
>De in dit artikel beschreven beste praktijken zijn niet uitputtend en zijn niet bedoeld om uw eigen veiligheidsbeleid en procedures te vervangen.

## Algemene beste praktijken

- Begin met de [&#x200B; geadviseerde reeks &#x200B;](./overview.md#adobe-recommended-rules) van standaard verkeersfilter en de regels van WAF die door Adobe worden verstrekt, en tweak hen die op de specifieke behoeften van uw toepassing en bedreigings landschap worden gebaseerd.
- Werk samen met uw veiligheidsteam om te bepalen welke regels zich op de de veiligheidshouding en nalevingsvereisten van uw organisatie richten.
- Test altijd nieuwe of bijgewerkte regels in ontwikkelomgevingen voordat u ze promoot naar werkgebied en productie.
- Wanneer u regels declareert en valideert, begint u met `action` type `log` om gedrag waar te nemen zonder legitiem verkeer te blokkeren.
- Ga van `log` naar `block` alleen nadat u voldoende verkeersgegevens hebt geanalyseerd en hebt bevestigd dat er geen geldige aanvragen worden beïnvloed.
- Introduceer regels incrementeel, die QA, prestaties, en veiligheidstestteams impliceren om onbedoelde bijwerkingen te identificeren.
- Regelmatige overzicht en analyseer regeldoeltreffendheid gebruikend [&#x200B; dashboard het tooling &#x200B;](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). De frequentie van de controle (dagelijks, wekelijks, maandelijks) dient overeen te stemmen met het verkeersvolume en het risicoprofiel van uw site.
- Verfijn onophoudelijk regels die op nieuwe bedreigingsintelligentie, verkeersgedrag, en controleresultaten worden gebaseerd.

## Beste werkwijzen voor verkeersfilterregels

- Het gebruik Adobe [&#x200B; geadviseerde standaardregels van de verkeersfilter &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules) als basislijn, die regels voor rand, oorsprongbescherming, en op OFAC-Gebaseerde beperkingen omvat.
- Herzie regelmatig alarm en logboeken om patronen van misbruik of misconfiguratie te identificeren.
- Pas drempelwaarden voor tarieflimieten aan op basis van de verkeerspatronen en het gebruikersgedrag van uw toepassing.

  Zie de volgende tabel voor hulp bij het kiezen van drempelwaarden:

  | Variatie | Waarde |
  | :--------- | :------- |
  | Oorsprong | Neem de hoogste waarde van de Max Verzoeken van de Oorsprong per IP/POP onder **normale** verkeersvoorwaarden (namelijk niet het tarief op het tijdstip van een DDoS) en verhoog het met een veelvoud |
  | Edge | Neem de hoogste waarde van de Max Verzoeken van Edge per IP/POP onder **normale** verkeersvoorwaarden (namelijk niet het tarief op het tijdstip van een DDoS) en verhoog het met een veelvoud |

  Zie ook [&#x200B; het kiezen drempelwaarden &#x200B;](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) sectie voor meer details.

- Ga pas naar `block` actie nadat u hebt bevestigd dat de `log` actie het legitieme verkeer niet beïnvloedt.

## Aanbevolen procedures voor WAF-regels

- Begin met Adobe [&#x200B; geadviseerde de regels van WAF &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules), die regels voor het blokkeren van bekende slechte IPs, het ontdekken van aanvallen DDoS, en het verlichten van beide misbruik omvatten.
- De markering `ATTACK` WAF moet u waarschuwen voor mogelijke bedreigingen. Zorg ervoor dat er geen valse positieven zijn voordat u naar `block` gaat.
- Als de aanbevolen WAF-regels geen betrekking hebben op specifieke bedreigingen, kunt u aangepaste regels maken op basis van de unieke vereisten van uw toepassing. Zie een volledige lijst van [&#x200B; de vlaggen van WAF &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) in de documentatie.

## Uitvoeringsbepalingen

Leer hoe te om de regels van de verkeersfilter en WAF in AEM as a Cloud Service uit te voeren:

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="AEM-websites beveiligen met de standaardregels voor verkeersfilters" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="AEM-websites beveiligen met de standaardregels voor verkeersfilters"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="AEM-websites beveiligen met de standaardregels voor verkeersfilters"> Beschermend de websites van AEM gebruikend de standaardregels van de verkeersfilter </a>
                    </p>
                    <p class="is-size-6">Leer hoe u AEM-websites kunt beschermen tegen DoS, DDoS en beide misbruiken met behulp van door Adobe aanbevolen standaardverkeersfilterregels in AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> pas Regels </span> toe
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="AEM-websites beschermen met WAF-regels" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="AEM-websites beschermen met WAF-regels"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="AEM-websites beschermen met WAF-regels"> Beschermend de websites van AEM gebruikend de regels van WAF </a>
                    </p>
                    <p class="is-size-6">Leer hoe u AEM-websites kunt beschermen tegen geavanceerde bedreigingen, zoals DoS, DDoS en beide misbruiken met de WAF-regels (Web Application Firewall) die door Adobe worden aanbevolen in AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> activeer WAF </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Aanvullende bronnen

- [&#x200B; Regels van de Filter van het Verkeer met inbegrip van de Regels van WAF &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [&#x200B; Begrijpend preventie DoS/DDoS in AEM &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [&#x200B; het Blokkeren Dos en aanvallen DDoS gebruikend de regels van de verkeersfilter &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

