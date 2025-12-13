---
title: De module ui.frontend van het volledige-stapelproject van het overzicht
description: Herzie de front-end ontwikkeling, plaatsing, en leveringslevenscyclus van een op beproefd-gebaseerd volledig-stapel AEM Sites Project.
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 364
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Bekijk de module &#39;ui.frontend&#39; van het AEM-project in volledige stapel {#aem-full-stack-ui-frontent}

In, herziet dit hoofdstuk wij de ontwikkeling, de plaatsing, en de levering van front-end artefacten van een volledig-stapel AEM project, door zich op de module &quot;ui.frontend&quot;van het __project van de Plaatsen WKND__ te concentreren.


## Doelstellingen {#objective}

* Begrijp de bouw en plaatsingsstroom van front-end artefacten in een volledig-stapelproject van AEM
* Herzie het volledige-stapelproject van AEM `ui.frontend` module [&#x200B; webpack &#x200B;](https://webpack.js.org/) vormt
* AEM-clientbibliotheekproces (ook clientlibs genoemd)

## Voorste-end plaatsingsstroom voor AEM full-stack en Snelle projecten van de Plaats creatie

>[!IMPORTANT]
>
>Deze video verklaart en toont de front-end stroom voor zowel **volledig-Stapel als Snelle projecten van de Aanmaak van de Plaats** aan om het subtiele verschil in de front-end middelen te schetsen bouwt, opstelt, en leveringsmodel.

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## Vereisten {#prerequisites}


* Kloon het [&#x200B; project van de Plaatsen van AEM WKND &#x200B;](https://github.com/adobe/aem-guides-wknd)
* Het gekloonde AEM WKND Sites-project is gemaakt en geïmplementeerd in AEM as a Cloud Service.

Zie het project van de Plaats van AEM WKND [&#x200B; README.md &#x200B;](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) voor meer details.

## AEM full-stack project front-end artefactflow {#flow-of-frontend-artifacts}

Hieronder is een vertegenwoordiging op hoog niveau van de __ontwikkeling, plaatsing, en levering__ stroom van de front-end artefacten in een volledig-stapel AEM project.

![&#x200B; Ontwikkeling, Plaatsing en Levering van Voorste-Eind Artefacten &#x200B;](assets/Dev-Deploy-Delivery-AEM-Project.png)


Tijdens de ontwikkelingsfase worden front-end wijzigingen zoals opmaak en herbranding uitgevoerd door de CSS- en JS-bestanden uit de map `ui.frontend/src/main/webpack` bij te werken. Dan tijdens bouwstijl-tijd, [&#x200B; webpack &#x200B;](https://webpack.js.org/) module-bundelaar en maven stop zet deze dossiers in geoptimaliseerde AEM clientlibs onder `ui.apps` module.

Voorste-eindveranderingen worden opgesteld aan het milieu van AEM as a Cloud Service wanneer het runnen van de [__volledig-stapel__ pijpleiding in Cloud Manager &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=nl-NL).

De front-end bronnen worden aan de webbrowsers geleverd via URI-paden die beginnen met `/etc.clientlibs/` en worden doorgaans in cache geplaatst op AEM Dispatcher en CDN.


>[!NOTE]
>
> Op dezelfde manier in de __Reis van de Aanmaak van de Plaats van AEM Snelle__, [&#x200B; front-end veranderingen &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html?lang=nl-NL) worden opgesteld aan het milieu van AEM as a Cloud Service door de __voor-Eind__ pijpleiding in werking te stellen, zie [&#x200B; Opstelling Uw Pijpleiding &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html?lang=nl-NL)

### Webpack van het overzicht vormt in het project van Plaatsen WKND {#development-frontend-webpack-clientlib}

* Er zijn drie __webpack__ config- dossiers die worden gebruikt om de de plaatsen van WKND front-end middelen te bundelen.

   1. `webpack.common` - dit bevat de __gemeenschappelijke__ configuratie om het middel te instrueren WKND bundelen en optimalisering. Het __output__ bezit vertelt waar te om de geconsolideerde dossiers (die ook als de bundels van JavaScript worden bekend, maar niet om met de bundels van AEM OSGi worden verward) uit te geven het leidt. De standaardnaam wordt ingesteld op `clientlib-site/js/[name].bundle.js` .

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` bevat de __ontwikkeling__ configuratie voor webpack-dev-server en richt aan het malplaatje van HTML aan gebruik. Het bevat ook een proxyconfiguratie voor een AEM-instantie die op `localhost:4502` wordt uitgevoerd.

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` bevat de __productie__ configuratie en gebruikt de plugins om de ontwikkelingsdossiers in geoptimaliseerde bundels om te zetten.

  ```javascript
      ...
      module.exports = merge(common, {
          mode: 'production',
          optimization: {
              minimize: true,
              minimizer: [
                  new TerserPlugin(),
                  new CssMinimizerPlugin({ ...})
          }
      ...    
  ```


* De gebundelde middelen worden verplaatst naar de `ui.apps` module die [&#x200B; aem-client-clientlib-generator &#x200B;](https://www.npmjs.com/package/aem-clientlib-generator) stop gebruikt, die de configuratie gebruikt in het `clientlib.config.js` dossier wordt beheerd.

```javascript
    ...
    const BUILD_DIR = path.join(__dirname, 'dist');
    const CLIENTLIB_DIR = path.join(
    __dirname,
    '..',
    'ui.apps',
    'src',
    'main',
    'content',
    'jcr_root',
    'apps',
    'wknd',
    'clientlibs'
    );
    ...
```

* De __front-maven-stop__ van `ui.frontend/pom.xml` organiseert webpack bundelen en clientlib generatie tijdens het project van AEM bouwt.

`$ mvn clean install -PautoInstallSinglePackage`

### Implementatie naar AEM as a Cloud Service {#deployment-frontend-aemaacs}

De [__volledig-stapel__ pijpleiding &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=nl-NL&#full-stack-pipeline) stelt deze veranderingen in een milieu van AEM as a Cloud Service op.


### Levering vanuit AEM as a Cloud Service {#delivery-frontend-aemaacs}

De front-end middelen die via de full-stack pijplijn worden opgesteld worden geleverd van de Plaats van AEM aan Webbrowsers als `/etc.clientlibs` dossiers. U kunt dit verifiëren door de [&#x200B; openbaar ontvangen plaats van WKND &#x200B;](https://wknd.site/content/wknd/us/en.html) en het bekijken bron van webpage te bezoeken.

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, hebt u de module ui.frontend van het full-stack project gecontroleerd

## Volgende stappen {#next-steps}

In het volgende hoofdstuk, [&#x200B; Update Project om Voorste-eindpijpleiding &#x200B;](update-project.md) te gebruiken, zult u het Project van de Plaatsen van AEM WKND bijwerken om het voor het front-end pijpleidingscontract toe te laten.
