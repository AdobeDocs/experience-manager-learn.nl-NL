---
title: Personalization - Overzicht
description: Leer hoe u AEM as a Cloud Service-websites kunt personaliseren met Adobe Target- en Adobe Experience Platform-toepassingen.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18717
thumbnail: null
exl-id: c4fb11b9-b613-4522-b9da-18d7ae0826ec
source-git-commit: 055dc7d666d082244d73d3494bac54d7eb4bb886
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---

# Personalization - Overzicht

Leer hoe AEM as a Cloud Service (AEMCS) integreert met Adobe Target en Adobe Experience Platform (AEP) om persoonlijke ervaringen te bieden. Gebruikend de Fragmenten van de Ervaring als gepersonaliseerde inhoud, ontdek hoe te om tests A/B in werking te stellen, doelgebruikers die op gedrag in real time worden gebaseerd, of verpersoonlijk inhoud gebruikend verenigde klantenprofielen die van gegevens over systemen worden gebouwd.

## Vereisten

Om diverse verpersoonlijkingsscenario&#39;s aan te tonen, gebruikt dit leerprogramma het steekproef [&#x200B; AEM WKND &#x200B;](https://github.com/adobe/aem-guides-wknd/) project. U hebt de volgende stappen nodig:

- Een Adobe org met toegang tot:
   - **milieu van AEM as a Cloud Service** - om inhoud tot stand te brengen en te beheren
   - **Adobe Target** - om gepersonaliseerde ervaringen samen te stellen en te leveren
   - **de toepassingen van Adobe Experience Platform** - om klantenprofielen en publiek te beheren
   - **Markeringen (vroeger Lancering) in AEP** - om het Web SDK en douane JavaScript voor gegevensinzameling en verpersoonlijking op te stellen

- Basiskennis van AEM-componenten en Experience Fragments

- Het [&#x200B; AEM WKND &#x200B;](https://github.com/adobe/aem-guides-wknd/) project dat aan uw milieu van AEM as a Cloud Service wordt opgesteld.

## Live demo van Personalization-gebruiksgevallen

De verpersoonlijking van de ervaring in actie op de [&#x200B; website van Enablement WKND &#x200B;](https://wknd.enablementadobe.com/us/en.html){target="_blank"}. De demo-site demonstreert drie typen personalisatie: A/B-tests, gedragsgericht gebruik en bekende gebruikersidentificatie.

>[!TIP]
>
> Door de live demo te verkennen, begrijpt u eerst de waarde en mogelijkheden van elke personalisatietechniek voordat u tijd investeert in installatie en implementatie.

<!-- CARDS
{target = _self}

* ./live-demo.md
  {title = Live Demo of Personalization Use Cases}
  {description = Experience personalization in action on the [WKND Enablement website](https://wknd.enablementadobe.com/us/en.html). The demo site demonstrates three types of personalization: A/B testing, behavioral targeting, and known-user personalization.}
  {image = ./assets/live-demo/live-demo.png}
  {cta = Live Demo}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Live Demo of Personalization Use Cases">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./live-demo.md" title="Live demo van Personalization-gebruiksgevallen" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/live-demo/live-demo.png" alt="Live demo van Personalization-gebruiksgevallen"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./live-demo.md" target="_self" rel="referrer" title="Live demo van Personalization-gebruiksgevallen"> Levende Demo van de Gevallen van het Gebruik van Personalization </a>
                    </p>
                    <p class="is-size-6">Ervaar personalisatie in actie op de WKND Enablement website. De demo-site demonstreert drie typen personalisatie: A/B-tests, gedragsgericht gebruik en bekende gebruikersidentificatie.</p>
                </div>
                <a href="./live-demo.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Levende Demo </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Aan de slag

Voordat u specifieke gebruiksgevallen gaat verkennen, configureert u eerst AEM as a Cloud Service voor personalisatie. Begin door Adobe Target en Markeringen te integreren om cliënt-zijpersonalisatie toe te laten gebruikend het Web SDK. Met deze basisstappen ondersteunen uw AEM-pagina&#39;s experimenten, doelgroepen en realtime personalisatie.

<!-- CARDS
{target = _self}

* ./setup/integrate-adobe-target.md
  {title = Integrate Adobe Target}
  {description = Integrate AEMCS with Adobe Target to activate personalized content, such as Experience Fragments, as offers.}
  {image = ./assets/setup/integrate-target.png}
  {cta = Integrate Target}

* ./setup/integrate-adobe-tags.md
  {title = Integrate Tags}
  {description = Integrate AEMCS with Tags to inject the Web SDK and custom JavaScript for data collection and personalization.}
  {image = ./assets/setup/integrate-tags.png}
  {cta = Integrate Tags}
  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Adobe Target">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-target.md" title="Adobe Target integreren" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-target.png" alt="Adobe Target integreren"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" title="Adobe Target integreren"> integreer Adobe Target </a>
                    </p>
                    <p class="is-size-6">Integreer AEMCS met Adobe Target om gepersonaliseerde inhoud, zoals de Fragmenten van de Ervaring, als aanbiedingen te activeren.</p>
                </div>
                <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> integreer Doel </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-tags.md" title="Tags integreren" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-tags.png" alt="Tags integreren"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" title="Tags integreren"> integreer Markeringen </a>
                    </p>
                    <p class="is-size-6">Integreer AEMCS met Tags om het Web SDK en de aangepaste JavaScript te injecteren voor gegevensverzameling en personalisatie.</p>
                </div>
                <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> integreer Markeringen </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## Gevallen gebruiken

Ontdek de volgende veelvoorkomende gebruiksgevallen voor personalisatie die worden ondersteund door AEMCS, Adobe Target en Adobe Experience Platform.

<!-- CARDS
{target = _self}

* ./use-cases/experimentation.md
    {title = Experimentation (A/B Testing)}
    {description = Learn how to test different content variations on an AEMCS website using Adobe Target for A/B testing.}
    {image = ./assets/use-cases/experiment/experimentation.png}
    {cta = Learn Experimentation}

* ./use-cases/behavioral-targeting.md
    {title = Behavioral Targeting}
    {description = Learn how to personalize content based on user behavior using Adobe Experience Platform and Adobe Target.}
    {image = ./assets/use-cases/behavioral-targeting/behavioral-targeting.png}
    {cta = Learn Behavioral Targeting}

* ./use-cases/known-user-personalization.md
    {title = Known-user personalization}
    {description = Learn how to personalize content based on known user data by stitching information from multiple systems into a complete customer profile.}
    {image = ./assets/use-cases/known-user-personalization/known-user-personalization.png}
    {cta = Learn Known-user personalization}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Experimentation (A/B Testing)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/experimentation.md" title="Experimentatie (A/B-test)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/experiment/experimentation.png" alt="Experimentatie (A/B-test)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/experimentation.md" target="_self" rel="referrer" title="Experimentatie (A/B-test)"> Experimentatie (het Testen A/B) </a>
                    </p>
                    <p class="is-size-6">Leer hoe u verschillende inhoudvariaties op een AEMCS-website test met Adobe Target voor A/B-tests.</p>
                </div>
                <a href="./use-cases/experimentation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer Experimentatie </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Behavioral Targeting">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/behavioral-targeting.md" title="Gedragingen" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/behavioral-targeting/behavioral-targeting.png" alt="Gedragingen"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/behavioral-targeting.md" target="_self" rel="referrer" title="Gedragingen"> Gedrag dat richt </a>
                    </p>
                    <p class="is-size-6">Leer hoe u inhoud kunt aanpassen op basis van gebruikersgedrag met Adobe Experience Platform en Adobe Target.</p>
                </div>
                <a href="./use-cases/behavioral-targeting.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer Gedrag dat richt </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Known-user personalization">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/known-user-personalization.md" title="Verpersoonlijking van bekende gebruikers" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/known-user-personalization/known-user-personalization.png" alt="Verpersoonlijking van bekende gebruikers"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/known-user-personalization.md" target="_self" rel="referrer" title="Verpersoonlijking van bekende gebruikers"> Bekende-gebruikersverpersoonlijking </a>
                    </p>
                    <p class="is-size-6">Leer hoe u inhoud kunt aanpassen op basis van bekende gebruikersgegevens door informatie van meerdere systemen te koppelen aan een volledig klantprofiel.</p>
                </div>
                <a href="./use-cases/known-user-personalization.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer Bekende-gebruikersverpersoonlijking </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
