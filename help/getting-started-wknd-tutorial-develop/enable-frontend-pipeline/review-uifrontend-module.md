---
title: De module ui.frontend van het volledige-stapelproject van het overzicht
description: Herzie de front-end ontwikkeling, plaatsing, en leveringslevenscyclus van een op beproefd-gebaseerd volledig-stapel AEM Sites Project.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 364
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Bekijk de module &#39;ui.frontend&#39; van het AEM &#39;full-stack&#39; project {#aem-full-stack-ui-frontent}

In, herziet dit hoofdstuk wij de ontwikkeling, de plaatsing, en de levering van front-end artefacten van een volledig-stapel AEM project, door zich op de module &quot;ui.frontend&quot;van het __project van de Plaatsen WKND__ te concentreren.


## Doelstellingen {#objective}

* Begrijp de bouw en plaatsingsstroom van front-end artefacten in een AEM volledig-stapelproject
* Herzie de AEM volledig-stapel van het project `ui.frontend` module [ webpack ](https://webpack.js.org/) vormt
* AEM clientbibliotheekproces (ook clientlibs genoemd)

## Voorste-end plaatsingsstroom voor AEM volledig-stapel en Snelle projecten van de Plaats

>[!IMPORTANT]
>
>Deze video verklaart en toont de front-end stroom voor zowel **volledig-Stapel als Snelle projecten van de Aanmaak van de Plaats** aan om het subtiele verschil in de front-end middelen te schetsen bouwt, opstelt, en leveringsmodel.

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## Vereisten {#prerequisites}


* Kloon het [ AEM WKND project van Plaatsen ](https://github.com/adobe/aem-guides-wknd)
* Het gekloonde AEM WKND-siteproject is gemaakt en geïmplementeerd in AEM as a Cloud Service.

Zie het AEM project van de Plaats WKND [ README.md ](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) voor meer details.

## AEM volledige-stapel project front-end artefactstroom {#flow-of-frontend-artifacts}

Hieronder is een vertegenwoordiging op hoog niveau van de __ontwikkeling, plaatsing, en levering__ stroom van de front-end artefacten in een volledig-stapel AEM project.

![ Ontwikkeling, Plaatsing en Levering van Voorste-Eind Artefacten ](assets/Dev-Deploy-Delivery-AEM-Project.png)


Tijdens de ontwikkelingsfase worden front-end wijzigingen zoals opmaak en herbranding uitgevoerd door de CSS- en JS-bestanden uit de map `ui.frontend/src/main/webpack` bij te werken. Dan tijdens bouwstijl-tijd, [ webpack ](https://webpack.js.org/) module-bundelaar en maven stop zet deze dossiers in geoptimaliseerde AEM clientlibs onder `ui.apps` module.

Voorste-eindveranderingen worden opgesteld aan het milieu van AEM as a Cloud Service wanneer het runnen van de [__volledig-stapel__ pijpleiding in Cloud Manager ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

De front-end middelen worden geleverd aan Webbrowsers via de wegen van URI die met `/etc.clientlibs/` beginnen, en typisch in het voorgeheugen ondergebracht op AEM Dispatcher en CDN.


>[!NOTE]
>
> Op dezelfde manier in de __AEM Snelle Reis van de Aanmaak van de Plaats__, [ front-end veranderingen ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) worden opgesteld aan het milieu van AEM as a Cloud Service door de __voor-Eind__ pijpleiding in werking te stellen, zie [ Opstelling Uw Pijpleiding ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### Webpack van het overzicht vormt in het project van Plaatsen WKND {#development-frontend-webpack-clientlib}

* Er zijn drie __webpack__ config- dossiers die worden gebruikt om de de plaatsen van WKND front-end middelen te bundelen.

   1. `webpack.common` - dit bevat de __gemeenschappelijke__ configuratie om het middel te instrueren WKND bundelen en optimalisering. Het __output__ bezit vertelt waar te om de geconsolideerde dossiers (die ook als de bundels van JavaScript worden bekend, maar niet om met AEM OSGi- bundels worden verward) uit te geven het leidt. De standaardnaam wordt ingesteld op `clientlib-site/js/[name].bundle.js` .

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` bevat de __ontwikkelings__ configuratie voor webpack-dev-server en richt aan het malplaatje van HTML aan gebruik. Het bevat ook een proxyconfiguratie voor een AEM instantie die op `localhost:4502` wordt uitgevoerd.

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


* De gebundelde middelen worden verplaatst naar de `ui.apps` module die [ aem-client-clientlib-generator ](https://www.npmjs.com/package/aem-clientlib-generator) stop gebruikt, die de configuratie gebruikt in het `clientlib.config.js` dossier wordt beheerd.

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

* De __front-maven-stop__ van `ui.frontend/pom.xml` organiseert webpack bundelen en clientlib generatie tijdens AEM project bouwt.

`$ mvn clean install -PautoInstallSinglePackage`

### Implementatie naar AEM as a Cloud Service {#deployment-frontend-aemaacs}

De [__volledig-stapel__ pijpleiding ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) stelt deze veranderingen in een milieu van AEM as a Cloud Service op.


### Levering vanuit AEM as a Cloud Service {#delivery-frontend-aemaacs}

De front-end middelen die via de full-stack pijpleiding worden opgesteld worden geleverd van de AEMPlaats aan Webbrowsers als `/etc.clientlibs` dossiers. U kunt dit verifiëren door de [ openbaar ontvangen plaats van WKND ](https://wknd.site/content/wknd/us/en.html) en het bekijken bron van webpage te bezoeken.

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

In het volgende hoofdstuk, [ Update Project om Voorste-eindpijpleiding ](update-project.md) te gebruiken, zult u het AEM Project van Plaatsen WKND bijwerken om het voor het front-end pijpleidingscontract toe te laten.
