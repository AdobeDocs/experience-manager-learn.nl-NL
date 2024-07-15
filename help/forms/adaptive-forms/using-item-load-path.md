---
title: Pad voor laden van items gebruiken om vervolgkeuzelijst te vullen
description: Een vervolgkeuzelijst configureren en vullen om waarden van een crx-knooppunt te lezen
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
exl-id: 89c486c8-95c3-4cd4-bf8e-a1b3558f17d6
duration: 34
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# Eigenschap voor objectladen in AEM Forms

Vorm en bevolk drop-down lijst gebruikend het bezit van de weg van de puntlading.
In het veld Pad laden item kan een auteur een URL opgeven waaruit de beschikbare opties in een vervolgkeuzelijst worden geladen.
Voer de onderstaande stappen uit om een dergelijk knooppunt in crx te maken:
* Aanmelden bij crx
* Maak een knooppunt met de naam assets (u kunt dit knooppunt naar wens een naam geven). Typ sling:map onder inhoud.
* Opslaan
* Klik op het nieuwe knooppunt met elementen en stel de eigenschappen ervan in zoals hieronder weergegeven
* U zult een bezit van typeKoord genoemd activa moeten tot stand brengen (u kunt het volgens uw vereiste noemen).Zorg ervoor het bezit een multivalue is. Geef de gewenste waarden op en sla deze op.
  ![ punt-lading-weg ](assets/item-load-path-crx.png)

Als u deze waarden in de vervolgkeuzelijst wilt laden, geeft u het volgende pad op in de eigenschap voor het laden van het pad van het item **/content/assets/assets/assettypes**

Het steekproefpakket kan [ van hier worden gedownload ](assets/item-load-path-package.zip)
