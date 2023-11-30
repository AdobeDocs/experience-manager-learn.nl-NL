---
title: Werk volledig-stapel AEM project bij om front-end pijpleiding te gebruiken
description: Leer hoe te om volledig-stapel AEM project bij te werken om het voor de front-end pijpleiding toe te laten, zodat bouwt het slechts en stelt de front-end artefacten op.
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
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# Werk volledig-stapel AEM project bij om front-end pijpleiding te gebruiken {#update-project-enable-frontend-pipeline}

In dit hoofdstuk, maken wij config veranderingen in __WKND-siteproject__ om de front-end pijpleiding te gebruiken om JavaScript en CSS op te stellen, eerder dan het vereisen van een volledige stapelpijpleiding uitvoering. Dit ontkoppelt de ontwikkeling en plaatsingslevenscyclus van front-end en back-end artefacten, die voor een sneller, iteratief ontwikkelingsproces over het algemeen toestaan.

## Doelstellingen {#objectives}

* Werk volledig-stapelproject bij om de front-end pijpleiding te gebruiken

## Overzicht van configuratieveranderingen in het full-stack AEM project

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## Vereisten {#prerequisites}

Dit is een meerdelige zelfstudie en er wordt van uitgegaan dat u de [Module &#39;ui.frontend&#39;](./review-uifrontend-module.md).


## Veranderingen in het full-stack AEM project

Er zijn drie project-verwante config veranderingen en een stijlverandering om voor een testlooppas op te stellen, zo in totaal vier specifieke veranderingen in het project WKND om het voor het front-end pijpleidingscontract toe te laten.

1. Verwijder de `ui.frontend` module van volledige-stapelbouwstijlcyclus

   * In, de wortel van het Project van de Plaatsen van WKND `pom.xml` de opmerking `<module>ui.frontend</module>` submodule-item.

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

   * En de opmerking heeft betrekking op afhankelijkheid van de `ui.apps/pom.xml`

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

1. De `ui.frontend` module voor het front-end pijpleidingscontract door twee nieuwe webpack configuratiedossiers toe te voegen.

   * Bestaande kopiëren `webpack.common.js` als `webpack.theme.common.js`en wijzigen `output` eigendom en `MiniCssExtractPlugin`, `CopyWebpackPlugin` config-parameters voor insteekmodule, zoals hieronder:

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
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * Bestaande kopiëren `webpack.prod.js` als `webpack.theme.prod.js`en wijzigt u de `common` locatie van variabele naar het bovenstaande bestand als

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


   * In de `package.json` bestand, zorg ervoor dat de  `name` eigenschapswaarde is gelijk aan de naam van de site `/conf` knooppunt. En onder de `scripts` eigenschap, `build` script dat aangeeft hoe de bestanden aan de voorzijde van deze module moeten worden gemaakt.

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

1. De `ui.content` module voor de front-end pijpleiding door twee Sling toe te voegen vormt.

   * Een bestand maken op `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - dit omvat alle front-end bestanden die `ui.frontend` module genereert onder de `dist` map met gebruik van webpack-constructieproces.

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
   >    De volledige versie bekijken [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) in de __AEM WKND-siteproject__.


   * Tweede `com.adobe.aem.wcm.site.manager.config.SiteConfig` met de `themePackageName` waarde gelijk aan `package.json` en `name` eigenschapswaarde en `siteTemplatePath` die verwijzen naar een `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` stub path value.

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
   >    Zie, de volledige [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) in de __AEM WKND-siteproject__.

1. Een thema of de stijlen veranderen om via front-end pijpleiding voor een testlooppas op te stellen, veranderen wij `text-color` aan Adobe rood (of u kunt uw kiezen) door bij te werken `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Zet deze wijzigingen tot slot door in de git-opslagplaats voor Adoben van uw programma.


>[!AVAILABILITY]
>
> Deze veranderingen zijn beschikbaar op GitHub binnen [__front-end pijpleiding__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) tak van de __AEM WKND-siteproject__.


## Voorzichtigheid - _Vooruiteinde pijplijn inschakelen_ knop

De [Spoorwegkiezer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) s [Site](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) geeft de optie **Vooruiteinde pijplijn inschakelen** als u de hoofdmap of sitepagina van uw site selecteert. Klikken **Vooruiteinde pijplijn inschakelen** De knop overschrijft het bovenstaande **Sling-configuraties**, zorg ervoor **u klikt niet** deze knop na het implementeren van de bovenstaande wijzigingen via de pijpleiding van Cloud Manager.

![Knop Pijl-vooruiteinde inschakelen](assets/enable-front-end-Pipeline-button.png)

Als er per ongeluk op wordt geklikt, moet u de pijpleidingen opnieuw uitvoeren om ervoor te zorgen dat het frontend pijpleidingscontract en de veranderingen worden hersteld.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, hebt u het project van Plaatsen WKND bijgewerkt om het voor het front-end pijpleidingscontract toe te laten.

## Volgende stappen {#next-steps}

In het volgende hoofdstuk: [Implementeren met behulp van de voorste pijplijn](create-frontend-pipeline.md), zult u een front-end pijpleiding creëren en in werking stellen en zult verifiëren hoe wij __weg__ van de &#39;/etc.clientlibs&#39; gebaseerde front-end resources levering.
