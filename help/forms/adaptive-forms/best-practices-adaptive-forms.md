---
title: Naamconventies en aanbevolen procedures voor het maken van aangepaste formulieren
seo-title: Naamconventies en aanbevolen procedures voor het maken van aangepaste formulieren
description: Naamconventies en aanbevolen procedures voor het maken van aangepaste formulieren
seo-description: Naamconventies en aanbevolen procedures voor het maken van aangepaste formulieren
feature: adaptive-forms
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 5b05dbe45babfcfcfc81995d9d48bc9b26b9b6c8
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 1%

---

# Best practices voor

Met Adobe Experience Manager (AEM)-formulieren kunt u complexe transacties transformeren in eenvoudige, prachtige digitale ervaringen. In het volgende document worden enkele aanvullende aanbevolen procedures beschreven die moeten worden gevolgd bij de ontwikkeling van Adaptive Forms. Dit document moet worden gebruikt in combinatie met [dit document](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Naamgevingsconventies

* **Deelvensters**
   * Deelvensternamen zijn hoofdletters en kleine letters die beginnen met een hoofdletter.

* **Formuliervelden**
   * Veldnamen zijn hoofdletters en kleine letters die beginnen met een kleine letter.
   * Start geen veldnamen met getallen
   * Geen streepjes &quot;-&quot; in uw namen opnemen. Deze zijn gelijk aan een minteken in uw code en fungeren als operatoren in uw code.
   * Namen kunnen letters, cijfers, onderstrepingstekens en dollartekens bevatten.
   * Namen moeten beginnen met een letter
   * Namen zijn hoofdlettergevoelig
   * Gereserveerde woorden (zoals JavaScript-trefwoorden) kunnen niet als namen worden gebruikt. Kijk uit voor andere af-specifieke gereserveerde woorden zoals   als &quot;paneel&quot;,&quot;naam&quot;.
   * Geen streepjes &quot;-&quot; in uw namen opnemen
* **Forms ontwikkelen**
   * Formulierfragmenten moeten in overweging worden genomen bij het ontwikkelen van grote vormen. Uitgestelde laden van formulierfragmenten inschakelen voor sneller laden   keer
   * **DataModel**
      * Aangepast gegevensmodel koppelen aan adaptief formulier wordt aanbevolen
   * **Objectgebeurtenissen**
      * Code met betrekking tot de zichtbaarheid van een object moet altijd in de zichtbaarheidsgebeurtenis van dat object worden geplaatst.
   * **Script**
      * Als de code die u in een adaptief formulier schrijft, meer dan 5 zichtbare regels overschrijdt, moet u de code naar een clientbibliotheek verplaatsen. In het ideale geval voegt u uw functie toe aan de clientbibliotheek en voegt u vervolgens de juiste jsdoc-tags toe, zodat de functie zichtbaar is in de editor voor Adaptieve formulierregels.


