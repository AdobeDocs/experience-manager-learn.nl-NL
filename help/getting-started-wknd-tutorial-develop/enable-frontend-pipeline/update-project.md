---
title: Werk volledig-stapel AEM project bij om front-end pijpleiding te gebruiken
description: Leer hoe te om volledig-stapel AEM project bij te werken om het voor de front-end pijpleiding toe te laten, zodat bouwt het slechts en stelt de front-end artefacten op.
sub-product: sites
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
source-git-commit: 2e3615e9e9305165ca9c3c93b38ac7e9bdcc51fb
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 0%

---


# Werk volledig-stapel AEM project bij om front-end pijpleiding te gebruiken {#update-project-enable-frontend-pipeline}

In dit hoofdstuk, maken wij config veranderingen in __WKND-siteproject__ om de front-end pijpleiding te gebruiken om JavaScript en CSS op te stellen, eerder dan het vereisen van een volledige stapelpijpleiding uitvoering. Hierdoor wordt de ontwikkelings- en implementatiefase van front-end en back-end artefacten losgekoppeld, waardoor een sneller, herhalend ontwikkelingsproces in het algemeen mogelijk wordt.

## Doelstellingen {#objectives}

* Werk volledig-stapelproject bij om de front-end pijpleiding te gebruiken

## Overzicht van configuratieveranderingen in het full-stack AEM project

>[!VIDEO](https://video.tv.adobe.com/v/3409419/)

## Vereisten {#prerequisites}

Dit is een meerdelige zelfstudie en er wordt van uitgegaan dat u de [Module &#39;ui.frontend&#39;](./review-uifrontend-module.md).


## Veranderingen in het full-stack AEM project

Er zijn drie project-verwante config veranderingen en een stijlverandering om voor een testlooppas op te stellen, zo in totaal vier specifieke veranderingen in het project WKND om het voor het front-end pijpleidingscontract toe te laten.

1. Verwijder de `ui.frontend` module van de volledige de bouwstijlcyclus van de stapel

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

   * Bestaande kopiëren `webpack.common.js` als `webpack.theme.common.js`, wijzigen `output` eigendom en `MiniCssExtractPlugin`, `CopyWebpackPlugin` config-parameters voor insteekmodule, zoals hieronder:

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'clientlib-[name]/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * Bestaande kopiëren `webpack.prod.js` als `webpack.theme.prod.js`en wijzigt u de `common` de locatie van de variabele naar het bovenstaande bestand als

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


   * In de `package.json` bestand, zorg ervoor dat de  `name` eigenschapswaarde is gelijk aan de naam van de site `/conf` knooppunt. En onder de `scripts` eigenschap, a `build` script dat aangeeft hoe de bestanden aan de voorzijde van deze module moeten worden gemaakt.

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

   * Een nieuw bestand maken op `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - dit omvat alle front-end bestanden die `ui.frontend` module genereert onder de `dist` map met gebruik van webpack-constructieproces.

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
   >    Zie, de volledige [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) in de __AEM WKND-siteproject__.


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

1. Een thema of de stijlen veranderen om via front-end pijpleiding voor een testlooppas op te stellen, veranderen wij `text-color` aan Adobe rood (of u kunt uw kiezen) door het bijwerken van `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Ten slotte moet u deze wijzigingen doorvoeren in de Adobe git-opslagplaats van uw programma.


>[!AVAILABILITY]
>
> Deze veranderingen zijn beschikbaar op GitHub binnen [__front-end pijpleiding__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) tak van de __AEM WKND-siteproject__.


## Gefeliciteerd! {#congratulations}

Gefeliciteerd, hebt u het project van Plaatsen WKND bijgewerkt om het voor het front-end pijpleidingscontract toe te laten.

## Volgende stappen {#next-steps}

In het volgende hoofdstuk: [Implementeren met behulp van de voorste pijplijn](create-frontend-pipeline.md), zult u een front-end pijpleiding creëren en in werking stellen en zult verifiëren hoe wij __weg__ van de &#39;/etc.clientlibs&#39; gebaseerde front-end resources levering.