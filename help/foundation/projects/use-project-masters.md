---
title: Hoe te om de Masters van het Project in AEM te gebruiken
description: De Masters van het project vereenvoudigen zeer gebruiker en teambeheer met AEM Projecten.
version: 6.4, 6.5, cloud-service
topic: Inhoudsbeheer, Samenwerking
feature: Projecten
level: Intermediate
role: User
kt: 256
thumbnail: 17740.jpg
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---


# Projectstramienen gebruiken

De Masters van het project vereenvoudigen zeer gebruiker en teambeheer met [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=12&learn=on)

Beheerders kunnen nu een **[!DNL Master Project]** maken en gebruikers toewijzen aan rollen/machtigingen als onderdeel van een projectteam. De projecten kunnen van een Master Project worden gecreeerd en automatisch het lidmaatschap van het Team erven. Dit biedt verschillende voordelen:

* Bestaande teams opnieuw gebruiken in meerdere projecten
* Versnelt projectverwezenlijking aangezien de Teams niet moeten opnieuw worden gecreeerd met de hand
* Teamlidmaatschap beheren vanaf een centrale locatie en eventuele updates voor Teams worden automatisch overgeërfd door projecten
* vermijdt verwezenlijking van dubbele ACLs die prestatieskwesties kan veroorzaken

[!DNL Master Projects] kan onder de  [!UICONTROL Masters] omslag onder worden gecreeerd  [!UICONTROL AEM Projects]. Zodra een Master Project wordt gecreeerd, toont het als optie naast beschikbare malplaatjes in de tovenaar wanneer de nieuwe Projecten worden gecreeerd.

[!DNL Project Masters] URL (lokale instantie AEM-auteur):  [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Verwijderen [!DNL Project Masters]

Het schrappen van een master project resulteert in onbruikbaar afgeleide projecten.

Alvorens een master project te schrappen, zorg ervoor dat alle afgeleide projecten worden gebeëindigd en uit AEM verwijderd. Zorg ervoor om het even welke vereiste projectgegevens te bewaren alvorens de afgeleide projecten te verwijderen. Zodra alle afgeleide projecten uit AEM worden verwijderd, kan het master project veilig worden geschrapt.

## [!DNL Project Masters] markeren als inactief

Door de status van het master project in inactief in de eigenschappen van het project te veranderen, verdwijnen de inactieve master projecten uit de lijst van master projecten.

Om inactieve master projecten te tonen, knevel de &quot;show actieve&quot;filterknoop in de hoogste bar (naast de knevel van de lijstvertoning). Om het inactieve project actief te maken opnieuw, selecteer eenvoudig het inactieve master project, geef projecteigenschappen uit, en plaats het opnieuw om actief te zijn.

## [!DNL Project Masters] begrijpen

![Technische weergave van projectmeesters](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] het werk door een reeks AEM gebruikersgroepen (eigenaren, redacteur, en waarnemer) te bepalen en afgeleide Projecten toe te staan om die centraal bepaalde gebruikersgroepen van verwijzingen te voorzien en te hergebruiken.

Dit vermindert het totale aantal gebruikersgroepen die in AEM worden vereist. Vóór [!DNL Project Masters], leidde elk project tot 3 gebruikersgroepen met begeleidende ACEs om toestemming-ing af te dwingen, zo 100 projecten die 300 gebruikersgroepen voortbrachten. Projectstramienen staan een willekeurig aantal projecten toe om dezelfde drie groepen opnieuw te gebruiken, ervan uitgaande dat het gedeelde lidmaatschap is afgestemd op de zakelijke vereisten in het hele project.
