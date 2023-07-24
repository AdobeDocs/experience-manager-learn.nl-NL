---
title: Zelfstudie Analytics integreren met Commerce
description: Leer hoe u Analytics kunt integreren met Commerce.
solution: Analytics, Commerce
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="Integratie" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---


# Analyse integreren met handel

* Controleer of het account toegang heeft tot Adobe Analytics.

* Maak een project in Adobe Analytics.

* Maak een schema.
   * U hebt dit nodig om een keuze te maken uit de opties in latere stappen. Als u een schema wilt maken, zoekt u in de linkerkolom onder &quot;Gegevensbeheer&quot; naar Schema&#39;s. Klik nu linksboven op &quot;Schema maken&quot;. Selecteer XDM ExperienceEvent.
   * Klik links in Veldgroepen zoeken op Toevoegen
      * In de zoekopdracht kunt u filteren door `ExperienceEvent Commerce`
      * Zoeken naar `Adobe Analytics ExperienceEvent Commerce` en schakel het selectievakje in
      * Zorg ervoor dat u op de knop `Add field groups` in de rechterbovenhoek opslaan en doorgaan
* Creeer een dataset, hebt u dit nodig wanneer uw opstelling &quot;DataStream&quot;daarna.
   * Dataset staat in de linkerkolom &quot;Gegevensbeheer&quot; en zoekt naar &quot;Datasets&quot;.
   * Klik vervolgens in de rechterbovenhoek op Gegevensset maken. U creeert de dataset van het schema.
   * het eerder gemaakte schema zoeken en gebruiken
* Maak DataStream. U kunt aan het krijgen door de &quot;Inzameling van Gegevens in de linkerkolom&quot;te gebruiken en &quot;Datastreams&quot;te zoeken.
* Tabellen maken met deelvensters en segmenten. Dit is een manier om gecompliceerd te zijn voor deze zelfstudie. U hebt een ervaren analist nodig als hulp.


Tot slot navigeert u om uw rapport te bekijken, naar Experience.adobe.com uw werkruimteproject te vinden, de verbinding van het project te klikken u wenst te bekijken en u zou iets als dit beeld moeten zien

![Analytische screenshot van bepaalde handelsgegevens](./assets/analytics-commerce/analytics-screenshot-commerce-items.png)