---
title: Overzicht - AEM-websites beschermen
description: Leer hoe te om uw websites van AEM tegen DoS, DDoS en kwaadwillig verkeer te beschermen gebruikend de regels van de verkeersfilter, met inbegrip van zijn subcategorie van de regels van de Firewall van de Toepassing van het Web (WAF) in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 0%

---


# Overzicht - AEM-websites beschermen

Leer hoe te om uw websites van AEM tegen Ontkenning van de Dienst (Dos), Verspreid Ontkenning van de Dienst (DDoS), kwaadwillig verkeer en geavanceerde aanvallen te beschermen gebruikend **regels van de verkeersfilter**, met inbegrip van zijn subcategorie van **de Firewall van de Toepassing van het Web (WAF)** regels in AEM as a Cloud Service.

U leert ook over de verschillen tussen standaard verkeersfilter en het verkeersfilterregels van WAF, wanneer om hen te gebruiken, en hoe te beginnen met Adobe-geadviseerde regels.

>[!IMPORTANT]
>
> De regels van de het verkeersfilter van WAF vereisen een extra **bescherming WAF-DDoS** of **Verbeterde vergunning van de Veiligheid**. De standaardregels van de verkeersfilter zijn beschikbaar aan de klanten van Plaatsen en van Forms door gebrek.

## Inleiding tot verkeersbeveiliging in AEM as a Cloud Service

AEM as a Cloud Service gebruikt een geïntegreerde CDN-laag om de levering van uw website te beschermen en te optimaliseren. Één van de meest kritieke componenten van de laag CDN is de capaciteit om verkeersregels te bepalen en af te dwingen. Deze regels fungeren als een beschermend schild om uw site te beveiligen tegen misbruik, misbruik en aanvallen, zonder dat dit ten koste gaat van de prestaties.

De veiligheid van het verkeer is essentieel om uptime te handhaven, gevoelige gegevens te beschermen, en een naadloze ervaring voor wettige gebruikers te verzekeren. AEM biedt twee categorieën beveiligingsregels:

- **Standaard de regels van de verkeersfilter**
- **het verkeersfilterregels van de Firewall van de Toepassing van het Web (WAF)**

De regelreeksen helpen klanten gemeenschappelijke en gesofisticeerde Webbedreigingen verhinderen, lawaai van kwaadwillige of misleidende cliënten verminderen, en waarneming verbeteren door verzoekregistreren, het blokkeren en patroonopsporing.

## Verschil tussen standaard- en WAF-regels voor verkeersfilters

| Functie | Standaardregels voor verkeersfilters | WAF-regels voor verkeersfilters |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| Doel | Misbruik zoals DoS, DDoS, schrapen, of beide activiteit verhinderen | Detecteren en reageren op geavanceerde aanvalspatronen (bijvoorbeeld OWASP Top 10), die ook bescherming bieden tegen vlekken |
| Voorbeelden | Snelheidsbeperking, geo-blokkering, filter voor gebruikersagent | SQL injectie, XSS, bekende aanvalIPs |
| Flexibiliteit | Zeer configureerbaar via YAML | Zeer configureerbaar via YAML, met vooraf gedefinieerde WAF-vlaggen |
| Aanbevolen modus | Begin in de modus `log` en ga vervolgens naar de modus `block` | Begin in de modus `block` voor `ATTACK-FROM-BAD-IP` WAF-markering en `log` voor `ATTACK` WAF-markering en ga vervolgens naar de modus `block` voor beide |
| Implementatie | Gedefinieerd in YAML en geïmplementeerd via Cloud Manager Config Pipeline | Gedefinieerd in YAML met `wafFlags` en geïmplementeerd via Cloud Manager Config Pipeline |
| Licentie | Bij Sites- en Forms-licenties inbegrepen | **vereist bescherming WAF-DDoS of Verbeterde vergunning van de Veiligheid** |

De standaardregels van de verkeersfilter zijn nuttig om zaken-specifiek beleid, zoals tariefgrenzen of het blokkeren van specifieke gebieden af te dwingen, evenals het blokkeren van verkeer dat op verzoekeigenschappen en kopballen zoals IP adres, weg of gebruikersagent wordt gebaseerd.
De WAF-regels voor verkeersfilters bieden daarentegen een uitgebreide proactieve bescherming voor bekende web-exploits en aanvalsvectoren, en beschikken over geavanceerde intelligentie om valse positieven te beperken (d.w.z. om legitiem verkeer te blokkeren).
Om beide soorten regels te bepalen, gebruikt u de syntaxis YAML, zie [ Syntaxis van de Regels van de Filterregel van 0} Verkeer voor meer details.](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax)

## Wanneer en waarom gebruiken ze

**de standaard regels van de verkeersfilter van het Gebruik** wanneer:

- U wilt organisatie-specifieke grenzen, zoals IP tarief het vertragen toepassen.
- U bent zich bewust van specifieke patronen (bijvoorbeeld, kwaadwillige IP adressen, gebieden, kopballen) die het filtreren vereisen.

**de regels van de het verkeersfilter van WAF van het Gebruik** wanneer:

- U wilt uitvoerige, **pro-actieve bescherming** tegen wijdverspreide bekende aanvalspatronen (bijvoorbeeld, injectie, protocolmisbruik), evenals bekende kwaadwillige IPs, die uit deskundige gegevensbronnen wordt verzameld.
- U wilt kwaadwillige verzoeken ontkennen terwijl het beperken van de kans om wettig verkeer te blokkeren.
- U wilt de hoeveelheid inspanning beperken om tegen gemeenschappelijke en gesofisticeerde bedreigingen te verdedigen, door eenvoudige configuratieregels toe te passen.

Samen, verstrekken deze regels een defensie-in-diepte strategie die klanten van AEM as a Cloud Service toestaat om zowel proactieve als reactieve maatregelen te nemen in het beveiligen van hun digitale eigenschappen.

## Door Adobe aanbevolen regels

Adobe biedt aanbevolen regels voor standaardverkeersfilters en WAF-regels voor verkeersfilters om u te helpen uw AEM-sites snel te beveiligen.

- **Standaard de regels van de verkeersfilter** (beschikbaar door gebrek): De gemeenschappelijke misbruikscenario&#39;s van het adres zoals Dos, DDoS en beide aanvallen tegen **CDN rand**, **oorsprong**, of verkeer van gesanctioneerde landen.\
  Voorbeelden zijn:
   - Het tarief dat IPs beperkt die meer dan 500 verzoeken/seconde _bij de CDN rand_ maakt
   - Het tarief dat IPs beperkt die meer dan 100 verzoeken/seconde _bij de oorsprong_ maakt
   - Blokkeren van verkeer uit landen die door het Office of Foreign Assets Control (OFAC) zijn vermeld

- **de regels van de het verkeersfilter van WAF** (vereist toe:voegen-op vergunning): Verstrekt extra bescherming tegen verfijnde bedreigingen, met inbegrip van [ Top tien van OWASP ](https://owasp.org/www-project-top-ten/) bedreigingen zoals SQL injectie, dwars-plaats scripting (XSS), en andere aanvallen van de Webtoepassing.
Voorbeelden zijn:
   - Het blokkeren van verzoeken van bekende slechte IP adressen
   - Het registreren of het blokkeren van verdachte verzoeken die als aanvallen worden gemarkeerd

>[!TIP]
>
> Begin door de **Adobe-geadviseerde regels** toe te passen om van de veiligheidsdeskundigheid van Adobe en aan de gang zijnde updates te profiteren. Als uw zaken specifieke risico&#39;s of randgevallen heeft, of om het even welke valse positieven (het blokkeren van wettig verkeer) merkt, kunt u **douaneregels** bepalen of het gebrek uitbreiden dat wordt geplaatst om aan uw behoeften te voldoen.

## Aan de slag

Leer hoe te om de regels van de verkeersfilter, met inbegrip van de regels van WAF, in AEM as a Cloud Service te bepalen, op te stellen, te testen en te analyseren door de opstellingsgids en gebruiksgevallen hieronder te volgen. Dit geeft u de achtergrondkennis zodat kunt u de Adobe-geadviseerde regels later vertrouwend toepassen.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./assets/setup/rules-setup.png}
  {cta = Start Now}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="Hoe te om de regels van de verkeersfilter met inbegrip van de regels van WAF te plaatsen" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="Hoe te om de regels van de verkeersfilter met inbegrip van de regels van WAF te plaatsen"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="Hoe te om de regels van de verkeersfilter met inbegrip van de regels van WAF te plaatsen"> hoe te de regels van de opstellingsverkeersfilter met inbegrip van de regels van WAF </a>
                    </p>
                    <p class="is-size-6">Leer hoe te opstelling om, de resultaten van de regels van de verkeersfilter met inbegrip van de regels van WAF tot stand te brengen, op te stellen te testen en te analyseren.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Begin nu </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Installatiegids voor door Adobe aanbevolen regels

Deze gids verstrekt geleidelijke instructies aan opstelling en opstelling van Adobe-geadviseerde standaardverkeersfilter en de regels van de de verkeersfilter van WAF in uw milieu van AEM as a Cloud Service.

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF rules}
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
                    <p class="is-size-6">Leer hoe u AEM-websites kunt beschermen tegen geavanceerde bedreigingen, zoals DoS, DDoS en beide misbruiken met WAF (Adobe-Recommended Web Application Firewall), verkeersfilterregels in AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> activeer WAF </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Gevallen voor geavanceerd gebruik

Voor meer geavanceerde scenario&#39;s, kunt u de volgende gebruiksgevallen onderzoeken die aantonen hoe te om de regels van de filter van het douaneverkeer uit te voeren die op specifieke bedrijfsvereisten worden gebaseerd:

<!-- CARDS
{target = _self}

* ./how-to/request-logging.md

* ./how-to/request-blocking.md

* ./how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-logging.md" title="Bewaking van gevoelige verzoeken" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="Bewaking van gevoelige verzoeken"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="Bewaking van gevoelige verzoeken"> Bewaking gevoelige verzoeken </a>
                    </p>
                    <p class="is-size-6">Leer hoe te om gevoelige verzoeken te controleren door hen te registreren gebruikend de regels van de verkeersfilter in AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-blocking.md" title="Toegang beperken" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="Toegang beperken"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="Toegang beperken"> Beperkend toegang </a>
                    </p>
                    <p class="is-size-6">Leer hoe te om toegang te beperken door specifieke verzoeken te blokkeren gebruikend de regels van de verkeersfilter in AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-transformation.md" title="Verzoeken normaliseren" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="Verzoeken normaliseren"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="Verzoeken normaliseren"> het Normaliseren verzoeken </a>
                    </p>
                    <p class="is-size-6">Leer hoe te om verzoeken te normaliseren door hen te transformeren gebruikend de regels van de verkeersfilter in AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Aanvullende bronnen

- [ Regels van de Filter van het Verkeer met inbegrip van de Regels van WAF ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
