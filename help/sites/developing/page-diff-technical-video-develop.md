---
title: Ontwikkelen voor paginaverschil in AEM Sites
description: In deze video ziet u hoe u aangepaste stijlen voor de functionaliteit Paginaverschil van AEM Sites kunt gebruiken.
feature: Authoring
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
duration: 281
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Ontwikkelen voor paginaverschil {#developing-for-page-difference}

In deze video ziet u hoe u aangepaste stijlen voor de functionaliteit Paginaverschil van AEM Sites kunt gebruiken.

## Paginaverschilstijlen aanpassen {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>In deze video worden aangepaste CSS toegevoegd aan de clientbibliotheek we.Retail, waar deze wijzigingen moeten worden aangebracht in het AEM Sites-project van de klant; in de voorbeeldcode hieronder: `my-project` .

AEM-paginaverschil verkrijgt de OOTB CSS via een directe load van `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css` .

Wegens deze directe lading van CSS eerder dan het gebruiken van een categorie van de cliëntbibliotheek, moeten wij een ander injectiepunt voor de douanestijlen vinden, en dit douaneinjectiepunt is het auteursclientlib van het project.

Dit heeft het voordeel om deze de stijloverschrijvingen van douanestijlen toe te staan om huurspecifiek te zijn.

### Maak de ontwerpende clientlib klaar {#prepare-the-authoring-clientlib}

Zorg ervoor dat er een `authoring` clientlib voor uw project bestaat op `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Aangepaste CSS opgeven {#provide-the-custom-css}

Voeg aan de `authoring` clientlib a `css.txt` van het project toe dat naar minder dossier wijst dat de met voeten tredende stijlen zal verstrekken. [&#x200B; Minder &#x200B;](https://lesscss.org/) wordt aangewezen toe te schrijven aan zijn vele geschikte eigenschappen, met inbegrip van klasse-verpakken die in dit voorbeeld leveraged is.

```shell
base=./css

htmldiff.less
```

Maak het `less` -bestand dat de stijloverschrijvingen in `/apps/my-project/clientlibs/authoring/css/htmldiff.less` bevat en geef desgewenst de overtrekstijlen op.

```css
/* Wrap with body to gives these rules more specificity than the OOTB */
body {

    /* .html-XXXX are the styles that wrap text that has been added or removed */

    .html-added {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px limegreen;
    }

    .html-removed {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px red;
    }

    /* .cq-component-XXXX require !important as the class these are overriding uses it. */

    .cq-component-changed {
        border: 2px dashed #B9DAFF !important;
        border-radius: 8px;
    }
    
    .cq-component-moved {
        border: 2px solid #B9DAFF !important;
        border-radius: 8px;
    }

    .cq-component-added {
        border: 2px solid #CCEBB8 !important;
        border-radius: 8px;
    }

    .cq-component-removed {
        border: 2px solid #FFCCCC !important;
        border-radius: 8px;
    }
}
```

### De CSS van de ontwerpclient opnemen via de paginacomponent {#include-the-authoring-clientlib-css-via-the-page-component}

Neem de categorie client-libs die u gebruikt, direct vóór de tag `</head>` op in de basispagina van het project `/apps/my-project/components/structure/page/customheaderlibs.html` om ervoor te zorgen dat de stijlen worden geladen.

Deze stijlen moeten worden beperkt tot de modi [!UICONTROL Edit] en [!UICONTROL preview] WCM.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

Het eindresultaat van een diff&#39;d-pagina met de bovenstaande stijlen zou er als volgt uitzien (HTML toegevoegd en Component gewijzigd).

![&#x200B; verschil van de Pagina &#x200B;](assets/page-diff.png)

## Aanvullende bronnen {#additional-resources}

* [&#x200B; Download de wij.Retail steekproefplaats &#x200B;](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [&#x200B; Gebruikend de Bibliotheken van de Cliënt van AEM &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [&#x200B; Minder CSS Documentatie &#x200B;](https://lesscss.org/)
