---
title: Werk volledig-stapel AEM project bij om front-end pijpleiding te gebruiken
description: Leer hoe te om volledig-stapel AEM project bij te werken om het voor de front-end pijpleiding toe te laten, zodat bouwt het slechts en stelt de front-end artefacten op.
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
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
duration: 307
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---

# Werk volledig-stapel AEM project bij om front-end pijpleiding te gebruiken {#update-project-enable-frontend-pipeline}

In dit hoofdstuk, maken wij config veranderingen in het __project van Plaatsen WKND__ om de front-end pijpleiding te gebruiken om JavaScript en CSS op te stellen, eerder dan het vereisen van een volledige uitvoering van de stapelpijpleiding. Dit ontkoppelt de ontwikkeling en plaatsingslevenscyclus van front-end en back-end artefacten, die voor een sneller, iteratief ontwikkelingsproces over het algemeen toestaan.

## Doelstellingen {#objectives}

* Werk volledig-stapelproject bij om de front-end pijpleiding te gebruiken

## Overzicht van configuratieveranderingen in het full-stack AEM-project

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## Vereisten {#prerequisites}

Dit is een meerdelig leerprogramma en men veronderstelt dat u [&#x200B; &quot;ui.frontend&quot;Module &#x200B;](./review-uifrontend-module.md) hebt herzien.


## Wijzigingen in het AEM-project met volledige stapel

Er zijn drie project-verwante config veranderingen en een stijlverandering om voor een testlooppas op te stellen, zo in totaal vier specifieke veranderingen in het project WKND om het voor het front-end pijpleidingscontract toe te laten.

1. De module `ui.frontend` verwijderen uit de volledige constructiecyclus voor stapels

   * In de hoofdmap van het WKND-siteproject `pom.xml` vindt u een opmerking over de submodulevermelding van `<module>ui.frontend</module>` .

   ```xml
       ...
       <modules>
       <module>all</module>
       <module>core</module>
       <!--
       <module>ui.frontend</module>
       -->                
       <module>ui.apps</module>
       ...
   ```

   * En aan opmerkingen gerelateerde afhankelijkheid van de `ui.apps/pom.xml`

   ```xml
       ...
       <!-- ====================================================================== -->
       <!-- D E P E N D E N C I E S                                                -->
       <!-- ====================================================================== -->
           ...
       <!--
           <dependency>
               <groupId>com.adobe.aem.guides</groupId>
               <artifactId>aem-guides-wknd.ui.frontend</artifactId>
               <version>${project.version}</version>
               <type>zip</type>
           </dependency>
       -->    
       ...
   ```

1. Bereid de `ui.frontend` module voor het front-end pijpleidingscontract door twee nieuwe webpack configuratiedossiers toe te voegen.

   * Kopieer de bestaande eigenschappen `webpack.common.js` as `webpack.theme.common.js` en change `output` en `MiniCssExtractPlugin` , `CopyWebpackPlugin` plugin config params zoals hieronder:

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'theme/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './theme' }
           ]
       })
   ...
   ```

   * Kopieer de bestaande `webpack.prod.js` als `webpack.theme.prod.js` en wijzig als volgt de locatie van de variabele `common` naar het bovenstaande bestand

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >De bovenstaande twee configuratiewijzigingen in &#39;webpack&#39; moeten verschillende uitvoerbestanden en mapnamen hebben, zodat we eenvoudig onderscheid kunnen maken tussen clientlib (Full-stack) en thema&#39;s die worden gegenereerd (front-end), frontale artefacten van de pijplijn.
   >
   >Zoals u hebt geraden, kunnen de bovenstaande wijzigingen worden overgeslagen om ook bestaande webpack-configuraties te gebruiken, maar de onderstaande wijzigingen zijn vereist.
   >
   >Het is aan jou hoe je ze wilt benoemen of organiseren.


   * Zorg ervoor dat in het bestand `package.json` de waarde van de eigenschap `name` gelijk is aan de naam van de site uit het knooppunt `/conf` . En onder de eigenschap `scripts` voert u een `build` -script uit dat aangeeft hoe de front-end bestanden van deze module moeten worden gemaakt.

   ```javascript
       {
       "name": "wknd",
       "version": "1.0.0",
       ...
   
       "scripts": {
           "build": "webpack --config ./webpack.theme.prod.js"
       }
   
       ...
       }
   ```

1. Bereid de `ui.content` module voor de front-end pijpleiding door twee het Verdraaien vormen toe te voegen.

   * Maak een bestand in `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - dit omvat alle front-end bestanden die de module `ui.frontend` onder de map `dist` genereert met behulp van het webpack-constructieproces.

   ```xml
   ...
       <css
       jcr:primaryType="nt:unstructured"
       element="link"
       location="header">
       <attributes
           jcr:primaryType="nt:unstructured">
           <as
               jcr:primaryType="nt:unstructured"
               name="as"
               value="style"/>
           <href
               jcr:primaryType="nt:unstructured"
               name="href"
               value="/theme/site.css"/>
   ...
   ```

   >[!TIP]
   >
   >    Zie volledige [&#x200B; HtmlPageItemsConfig &#x200B;](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) in het __project van de Plaatsen van AEM WKND__.


   * Vervolgens wordt de waarde van `com.adobe.aem.wcm.site.manager.config.SiteConfig` met de waarde van `themePackageName` gelijk aan de waarde van de eigenschap `package.json` en `name` en `siteTemplatePath` gewijzigd, waarbij wordt verwezen naar de waarde van een `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` stub-pad.

   ```xml
   ...
       <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
               jcr:primaryType="nt:unstructured"
               siteTemplatePath="/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0"
               themePackageName="wknd">
       </jcr:root>
   ...
   ```

   >[!TIP]
   >
   >    Zie, volledige [&#x200B; SiteConfig &#x200B;](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) in het __project van de Plaatsen van AEM WKND__.

1. Als een thema of stijlen worden gewijzigd en via een front-end pijplijn voor een testrun worden geïmplementeerd, veranderen we `text-color` in Adobe rood (of u kunt uw eigen stijl kiezen) door de `ui.frontend/src/main/webpack/base/sass/_variables.scss` bij te werken.

   ```css
       $black:     #a40606;
       ...
   ```

Breng deze wijzigingen tot slot door naar de Adobe Git-opslagplaats van uw programma.


>[!AVAILABILITY]
>
> Deze veranderingen zijn beschikbaar op GitHub binnen de [__front-end pijpleiding__ &#x200B;](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) tak van het __project van de Plaatsen van AEM WKND__.


## De voorzichtigheid - _laat Voorste Pijl van het Eind_ knoop toe

De [&#x200B; Selector van het Spoorspoor &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) &quot;s [&#x200B; optie van de Plaats &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) toont **toelaten Voorste Pijpleiding van het Eind** op het selecteren van uw plaatswortel of plaatspagina. Het klikken **laat de Voorste knoop van het Eind toe** hierboven **het Schuiven vormt**, zorg ervoor **u** deze knoop na het opstellen boven veranderingen via de pijpleidingsuitvoering van Cloud Manager niet klikt.

![&#x200B; laat Voorste knoop van de Pijl van het Eind toe &#x200B;](assets/enable-front-end-Pipeline-button.png)

Als er per ongeluk op wordt geklikt, moet u de pijpleidingen opnieuw uitvoeren om ervoor te zorgen dat het frontend pijpleidingscontract en de veranderingen worden hersteld.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, hebt u het project van Plaatsen WKND bijgewerkt om het voor het front-end pijpleidingscontract toe te laten.

## Volgende stappen {#next-steps}

In het volgende hoofdstuk, [&#x200B; opstelt gebruikend de Voorste-Eind Pijpleiding &#x200B;](create-frontend-pipeline.md), zult u een front-end pijpleiding creëren en in werking stellen en zult verifiëren hoe wij __weg__ van de &quot;/etc.clientlibs&quot;gebaseerde front-end middelenlevering bewogen.
