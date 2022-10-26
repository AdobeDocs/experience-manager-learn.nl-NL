---
title: De module ui.frontend van het volledige-stapelproject van het overzicht
description: Herzie de front-end ontwikkeling, plaatsing, en leveringslevenscyclus van een op beproefd-gebaseerd volledig-stapel AEM Sites Project.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 0%

---


# Bekijk de module &#39;ui.frontend&#39; van het AEM &#39;full-stack&#39;-project {#aem-full-stack-ui-frontent}

In dit hoofdstuk bekijken we de ontwikkeling, implementatie en levering van front-end artefacten van een full-stack AEM project, door ons te richten op de &#39;ui.frontend&#39; module van de __WKND-siteproject__.


## Doelstellingen {#objective}

* Begrijp de bouw en plaatsingsstroom van front-end artefacten in een AEM volledig-stapelproject
* Herzie de AEM volledig-stapel project `ui.frontend` module [webpack](https://webpack.js.org/) configs
* AEM clientbibliotheekproces (ook clientlibs genoemd)

## Voorste-end plaatsingsstroom voor AEM volledig-stapel en Snelle projecten van de Plaats

>[!IMPORTANT]
>
>In deze video wordt de front-end flow voor beide uitgelegd en getoond **Volledig stapelen en snel site maken** projecten om het subtiele verschil in de front-end middelen te schetsen bouwen, opstellen, en leveringsmodel.

>[!VIDEO](https://video.tv.adobe.com/v/3409344/)

## Vereisten {#prerequisites}


* Klonen met [AEM WKND-siteproject](https://github.com/adobe/aem-guides-wknd)
* Ontwikkeld en geïmplementeerd het gekloonde AEM WKND Sites-project om as a Cloud Service te AEM.

Zie het AEM WKND-siteproject [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) voor meer informatie .

## AEM volledige-stapel project front-end artefactstroom {#flow-of-frontend-artifacts}

Hieronder ziet u een vertegenwoordiging op hoog niveau van de __ontwikkeling, implementatie en levering__ stroom van de front-end artefacten in een volledig-stapel AEM project.

![Ontwikkeling, implementatie en levering van front-end artefacten](assets/Dev-Deploy-Delivery-AEM-Project.png)


Tijdens de ontwikkelingsfase worden front-end wijzigingen zoals opmaak en herbranding uitgevoerd door de CSS- en JS-bestanden van de `ui.frontend/src/main/webpack` map. Tijdens de build-tijd [webpack](https://webpack.js.org/) module-bundelaar en gefabriceerde stop zet deze dossiers in geoptimaliseerde AEM clientlibs onder `ui.apps` module.

Voorste veranderingen worden opgesteld aan AEM as a Cloud Service milieu wanneer het runnen van [__Volledig stapelen__ pijplijn in Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

De front-end bronnen worden via URI-paden aan de webbrowsers geleverd, te beginnen met `/etc.clientlibs/`en worden doorgaans in het cachegeheugen opgeslagen op AEM Dispatcher en CDN.


>[!NOTE]
>
> Op dezelfde wijze __Reis voor snel maken van site AEM__ de [wijzigingen vooraf](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) worden geïmplementeerd in AEM as a Cloud Service omgeving door __Voorkant__ pijpleiding, zie [Uw pijplijn instellen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### Webpack van het overzicht vormt in het project van Plaatsen WKND {#development-frontend-webpack-clientlib}

* Er zijn drie __webpack__ configuratiebestanden die worden gebruikt om de front-end bronnen van de WKND-sites te bundelen.

   1. `webpack.common` - Dit bevat de __gemeenschappelijk__ configuratie om de WKND middelbundeling en optimalisering te instrueren. De __output__ Deze eigenschap vertelt waar de geconsolideerde bestanden (ook wel JavaScript-bundels genoemd, maar niet mogen worden verward met AEM OSGi-bundels) die worden gemaakt. De standaardnaam is ingesteld op `clientlib-site/js/[name].bundle.js`.

   ```javascript
       ...
       output: {
               filename: 'clientlib-site/js/[name].bundle.js',
               path: path.resolve(__dirname, 'dist')
           }
       ...    
   ```

   1. `webpack.dev.js` bevat de __ontwikkeling__ -configuratie voor de webpack-dev-server en wijst naar de HTML-sjabloon die moet worden gebruikt. Het bevat ook een volmachtsconfiguratie aan een AEM instantie die op loopt `localhost:4502`.

   ```javascript
       ...
       devServer: {
           proxy: [{
               context: ['/content', '/etc.clientlibs', '/libs'],
               target: 'http://localhost:4502',
           }],
       ...    
   ```

   1. `webpack.prod.js` bevat de __productie__ en gebruikt de plug-ins om de ontwikkelingsbestanden om te zetten in geoptimaliseerde bundels.

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


* De gebundelde bronnen worden verplaatst naar de `ui.apps` module gebruiken [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) insteekmodule, de configuratie gebruiken die in de `clientlib.config.js` bestand.

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

* De __frontend-maven-plugin__ van `ui.frontend/pom.xml` Orchestrates webpack bundling en clientlib generation tijdens AEM project build.

`$ mvn clean install -PautoInstallSinglePackage`

### Implementatie naar AEM as a Cloud Service {#deployment-frontend-aemaacs}

De [__Volledig stapelen__ pijpleiding](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) stelt deze veranderingen in een AEM as a Cloud Service milieu op.


### Levering van AEM as a Cloud Service {#delivery-frontend-aemaacs}

De front-end middelen die via de full-stack pijpleiding worden opgesteld worden geleverd van de AEMPlaats aan Webbrowsers als `/etc.clientlibs` bestanden. U kunt dit verifiëren door de [openbaar gehoste WKND-site](https://wknd.site/content/wknd/us/en.html) en weergavebron van de webpagina.

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

In het volgende hoofdstuk: [Project bijwerken voor gebruik van voorste pijplijn](update-project.md), zult u het AEM Project van de Plaatsen WKND bijwerken om het voor het front-end pijpleidingscontract toe te laten.
