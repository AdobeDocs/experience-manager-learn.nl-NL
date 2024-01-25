---
title: Naamconventies en aanbevolen procedures voor het maken van aangepaste formulieren
description: Naamconventies en aanbevolen procedures voor het maken van aangepaste formulieren
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
last-substantial-update: 2020-09-10T00:00:00Z
duration: 65
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---

# Aanbevolen procedures

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
   * Gereserveerde woorden (zoals JavaScript-trefwoorden) kunnen niet als namen worden gebruikt. Kijk uit voor andere AF-specifieke gereserveerde woorden zoals &quot;panel&quot;, &quot;name&quot;.
   * Geen streepjes &quot;-&quot; in uw namen opnemen
* **Forms ontwikkelen**
   * Formulierfragmenten moeten in overweging worden genomen bij het ontwikkelen van grote vormen. Het laden van formulierfragmenten vertragen voor snellere laadtijden
   * **DataModel**
      * Aangepast gegevensmodel koppelen aan adaptief formulier wordt aanbevolen

   * **Objectgebeurtenissen**
      * Code met betrekking tot de zichtbaarheid van een object moet altijd in de zichtbaarheidsgebeurtenis van dat object worden geplaatst.
   * **Script**
      * Als de code die u in een adaptief formulier schrijft, meer dan 5 zichtbare regels overschrijdt, moet u de code naar een clientbibliotheek verplaatsen. In het ideale geval voegt u uw functie toe aan de clientbibliotheek en voegt u vervolgens de juiste jsdoc-tags toe, zodat de functie zichtbaar is in de editor voor Adaptieve formulierregels.
