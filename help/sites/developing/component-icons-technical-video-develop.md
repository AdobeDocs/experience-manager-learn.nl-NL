---
title: Componentpictogrammen aanpassen in Adobe Experience Manager Sites
description: Met componentpictogrammen kunnen auteurs snel een component met pictogrammen of betekenisvolle afkortingen identificeren. Auteurs kunnen nu de Componenten vinden die nodig zijn om hun webervaringen sneller dan ooit uit te bouwen.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
duration: 379
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 0%

---

# Componentpictogrammen aanpassen {#developing-component-icons-in-aem-sites}

Met componentpictogrammen kunnen auteurs snel een component met pictogrammen of betekenisvolle afkortingen identificeren. Auteurs kunnen nu de Componenten vinden die nodig zijn om hun webervaringen sneller dan ooit uit te bouwen.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

De Componentbrowser wordt nu in een consistent grijs thema weergegeven, met daarin het volgende:

* **[!UICONTROL Component Group]**
* **[!UICONTROL Component Title]**
* **[!UICONTROL Component Description]**
* **[!UICONTROL Component Icon]**
   * De eerste twee letters van de Titel van de Component *(gebrek)*
   * Aangepaste PNG-afbeelding *(geconfigureerd door een ontwikkelaar)*
   * Aangepaste SVG-afbeelding *(geconfigureerd door een ontwikkelaar)*
   * Het pictogram CoralUI *(gevormd door een ontwikkelaar)*

## Opties voor configuratie van componentpictogrammen {#component-icon-configuration-options}

### Afkortingen {#abbreviations}

Door gebrek, worden de eerste 2 karakters van componententitel (**[cq:Component ]@jcr:title**) gebruikt als afkorting. Bijvoorbeeld, als **[cq:Component ]@jcr:title=Article Lijst** de afkorting als &quot;**Ar**&quot;zou tonen.

De afkorting kan via **[worden aangepast cq:Component ]@abbrelayback** bezit. Hoewel deze waarde meer dan 2 tekens kan bevatten, wordt aangeraden de afkorting te beperken tot 2 tekens om visuele stoornissen te voorkomen.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI-pictogrammen {#coralui-icons}

CoralUI-pictogrammen, geleverd door AEM, kunnen worden gebruikt voor componentpictogrammen. Om een pictogram CoralUI te vormen, plaats a **[cq:Component ]@cq:pictogram** bezit aan de gewenste het pictogramkenmerkwaarde van HTML van het pictogram CoralUI (die in de [ documentatie CoralUI ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html) wordt opgesomd.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG-afbeeldingen {#png-images}

PNG-afbeeldingen kunnen worden gebruikt voor componentpictogrammen. Om een beeld PNG als componentenpictogram te vormen, voeg het gewenste beeld als a **niet toe:dossier** genoemd **cq:icon.png** onder **[cq:Component]**.

PNG zou een transparante achtergrond moeten hebben, of een achtergrondkleur die aan **#707070** wordt geplaatst.

De beelden PNG worden geschraapt aan **20px door 20px**. Nochtans om netvliesvertoningen **40px** door **40px** aan te passen zou verkiesbaar kunnen zijn.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG-afbeeldingen {#svg-images}

SVG-afbeeldingen (op basis van vectoren) kunnen worden gebruikt voor componentpictogrammen. Om een beeld van SVG als componentenpictogram te vormen, voeg gewenste SVG als a **toe:dossier** genoemd **cq:icon.svg** onder **[cq:Component]**.

De beelden van SVG zouden een achtergrondkleur moeten hebben die aan **wordt geplaatst#707070** en een grootte van **20px door 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Aanvullende bronnen {#additional-resources}

* [ Beschikbare Pictogrammen CoralUI ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
