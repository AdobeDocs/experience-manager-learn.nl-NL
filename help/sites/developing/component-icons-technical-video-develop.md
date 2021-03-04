---
title: Componentpictogrammen aanpassen in Adobe Experience Manager Sites
description: Met componentpictogrammen kunnen auteurs snel een component met pictogrammen of betekenisvolle afkortingen identificeren. Auteurs kunnen nu de Componenten vinden die nodig zijn om hun webervaringen sneller dan ooit uit te bouwen.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
feature: Kernonderdelen
topic: Ontwikkeling
role: Zakelijke praktiserer
level: Intermediair
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 0%

---


# Componentpictogrammen aanpassen {#developing-component-icons-in-aem-sites}

Met componentpictogrammen kunnen auteurs snel een component met pictogrammen of betekenisvolle afkortingen identificeren. Auteurs kunnen nu de Componenten vinden die nodig zijn om hun webervaringen sneller dan ooit uit te bouwen.

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

De Componentbrowser wordt nu in een consistent grijs thema weergegeven, met daarin het volgende:

* **[!UICONTROL Component Group]**
* **[!UICONTROL Component Title]**
* **[!UICONTROL Component Description]**
* **[!UICONTROL Component Icon]**
   * De eerste twee letters van de Titel van de Component *(gebrek)*
   * Aangepaste PNG-afbeelding *(geconfigureerd door een ontwikkelaar)*
   * Aangepaste SVG-afbeelding *(geconfigureerd door een ontwikkelaar)*
   * CoralUI-pictogram *(geconfigureerd door een ontwikkelaar)*

## Configuratieopties voor componentpictogram {#component-icon-configuration-options}

### Afkortingen {#abbreviations}

Standaard worden de eerste 2 tekens van de componenttitel (**[cq:Component]@jcr:title**) gebruikt als de afkorting. Als bijvoorbeeld **[cq:Component]@jcr:title=Article List** de afkorting wordt weergegeven als &quot;**Ar**&quot;.

De afkorting kan worden aangepast via de eigenschap **[cq:Component]@abbreabonnement**. Hoewel deze waarde meer dan 2 tekens kan bevatten, wordt aangeraden de afkorting te beperken tot 2 tekens om visuele stoornissen te voorkomen.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI-pictogrammen {#coralui-icons}

Pictogrammen van CoralUI, opgegeven door AEM, kunnen worden gebruikt voor componentpictogrammen. Om een pictogram CoralUI te vormen, plaats een **[cq:Component]@cq:icon** bezit aan de gewenste het pictogramkenmerkwaarde van HTML van het pictogram CoralUI (opgesomd in [documentatie CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG-afbeeldingen {#png-images}

PNG-afbeeldingen kunnen worden gebruikt voor componentpictogrammen. Als u een PNG-afbeelding als componentpictogram wilt configureren, voegt u de gewenste afbeelding toe als een **nt:file** met de naam **cq:icon.png** onder **[cq:Component]**.

De PNG moet een transparante achtergrond hebben of een achtergrondkleur hebben die is ingesteld op **#707070**.

De PNG-afbeeldingen worden geschaald naar **20px bij 20px**. Als u echter ruimte wilt maken voor netvliesweergave **40px** van **40px**, heeft u wellicht de voorkeur.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG-afbeeldingen {#svg-images}

SVG-afbeeldingen (gebaseerd op vectoren) kunnen worden gebruikt voor componentpictogrammen. Als u een SVG-afbeelding als componentpictogram wilt configureren, voegt u de gewenste SVG toe als een **nt:file** met de naam **cq:icon.svg** onder **[cq:Component]**.

SVG-afbeeldingen moeten een achtergrondkleur hebben ingesteld op **#707070** en een grootte van **20px bij 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Aanvullende bronnen {#additional-resources}

* [Beschikbare CoralUI-pictogrammen](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
