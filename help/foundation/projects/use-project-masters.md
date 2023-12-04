---
title: In AEM gebruiken van Projectstramienen
description: De Masters van het project vereenvoudigen zeer gebruiker en teambeheer met AEM Projecten.
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
jira: KT-256
thumbnail: 17740.jpg
doc-type: Feature Video
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
duration: 288
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 0%

---

# Projectstramienen gebruiken

De Masters van het project vereenvoudigen zeer gebruiker en teambeheer met [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

Beheerders kunnen nu een **[!DNL Master Project]** en wijs gebruikers aan rollen/toestemmingen als deel van een Team van het Project toe. De projecten kunnen van een HoofdProject worden gecreeerd en automatisch het lidmaatschap van het Team erven. Dit biedt verschillende voordelen:

* Bestaande teams opnieuw gebruiken in meerdere projecten
* Versnelt projectverwezenlijking aangezien de Teams niet moeten opnieuw worden gecreeerd met de hand
* Teamlidmaatschap beheren vanaf een centrale locatie en eventuele updates voor Teams worden automatisch overgeërfd door projecten
* vermijdt verwezenlijking van dubbele ACLs die prestatieskwesties kan veroorzaken

[!DNL Master Projects] kunnen worden gecreëerd onder de [!UICONTROL Masters] map onder [!UICONTROL AEM Projects]. Zodra een HoofdProject wordt gecreeerd, toont het als optie naast beschikbare malplaatjes in de tovenaar wanneer de nieuwe Projecten worden gecreeerd.

[!DNL Project Masters] URL (lokale AEM instantie Auteur): [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Verwijderen [!DNL Project Masters]

Het schrappen van een hoofdproject resulteert in onbruikbaar afgeleide projecten.

Alvorens een hoofdproject te schrappen, zorg ervoor dat alle afgeleide projecten worden gebeëindigd en uit AEM verwijderd. Zorg ervoor om het even welke vereiste projectgegevens te bewaren alvorens de afgeleide projecten te verwijderen. Zodra alle afgeleide projecten uit AEM worden verwijderd, kan het hoofdproject veilig worden geschrapt.

## Markeren [!DNL Project Masters] als Inactief

Door de status van het hoofdproject in inactief in de eigenschappen van het project te veranderen, verdwijnen de inactieve hoofdprojecten uit de lijst van hoofdprojecten.

Om inactieve hoofdprojecten te tonen, knevel de &quot;show actieve&quot;filterknoop in de hoogste bar (naast de knevel van de lijstvertoning). Om het inactieve project actief te maken opnieuw, selecteer eenvoudig het inactieve hoofdproject, geef projecteigenschappen uit, en plaats het opnieuw om actief te zijn.

## Begrijpen [!DNL Project Masters]

![Technisch overzicht van projectmeesters](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] het werk door een reeks AEM gebruikersgroepen (eigenaren, redacteur, en waarnemer) te bepalen en afgeleide Projecten toe te staan om die centraal bepaalde gebruikersgroepen van verwijzingen te voorzien en te hergebruiken.

Dit vermindert het totale aantal gebruikersgroepen die in AEM worden vereist. Voor [!DNL Project Masters], leidde elk project tot 3 gebruikersgroepen met begeleidende ACEs om toestemming-ing af te dwingen, zo 100 projecten die 300 gebruikersgroepen voortbrachten. Projectstramienen staan een willekeurig aantal projecten toe om dezelfde drie groepen opnieuw te gebruiken, ervan uitgaande dat het gedeelde lidmaatschap is afgestemd op de zakelijke vereisten in het hele project.
