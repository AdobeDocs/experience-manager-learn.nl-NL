---
title: Adobe Stock-middelen gebruiken met AEM Assets
description: AEM biedt gebruikers de mogelijkheid om Adobe Stock-middelen rechtstreeks vanuit AEM te zoeken, voor te vertonen, op te slaan en te licentiëren. Organisaties kunnen hun Adobe Stock Enterprise-plan nu integreren met AEM Assets om ervoor te zorgen dat gelicentieerde middelen nu ruim beschikbaar zijn voor hun creatieve en marketingprojecten, met de krachtige mogelijkheden voor middelenbeheer van AEM.
feature: Adobe Stock
version: 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
doc-type: Feature Video
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
duration: 1104
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 0%

---

# Adobe Stock gebruiken met AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 biedt gebruikers de mogelijkheid om Adobe Stock-middelen rechtstreeks vanuit AEM te zoeken, voor te vertonen, op te slaan en in licentie te geven. Organisaties kunnen hun Adobe Stock Enterprise-plan nu integreren met AEM Assets om ervoor te zorgen dat gelicentieerde middelen nu ruim beschikbaar zijn voor hun creatieve en marketingprojecten, met de krachtige mogelijkheden voor middelenbeheer van AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678?quality=12&learn=on)

>[!NOTE]
>
>De integratie vereist een [Enterprise Adobe Stock-abonnement](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) en AEM 6.4 met minstens Service Pack 2 opgesteld. Zie voor AEM 6.4 service pack details [releaseopmerkingen](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html).

Dankzij de Adobe Stock- en AEM Assets-integratie kunnen auteurs en marketers van content eenvoudig een licentie voor en gebruik van stock assets maken voor creatieve of marketingdoeleinden. U kunt een zoekopdracht voor Stock-elementen uitvoeren met Omni Search, door het locatiefilter toe te voegen als Adobe Stock of door door de belangrijkste navigatie van AEM Assets te navigeren en op het UI-pictogram Zoeken in Adobe Stock Coral te klikken.

## Mogelijkheden

### Zoeken en opslaan

* Adobe Stock-middelen zoeken zonder AEM werkruimte te verlaten.
* Sla Adobe Stock-middelen op voor een voorvertoning, zonder dat u een licentie voor het element hoeft aan te schaffen.
* Mogelijkheid om Adobe Stock-middelen in licentie te geven en op te slaan naar AEM Assets
* Mogelijkheid om vergelijkbare middelen te zoeken vanuit Adobe Stock in de gebruikersinterface van AEM Assets
* Een geselecteerd middel van Stock Search in AEM Assets bekijken op de Adobe Stock-website
* Bestanden met licentiebronnen zijn gemarkeerd met een blauwe, licentie-badge voor eenvoudige identificatie

### Metagegevens van element

* In licentie gegeven middelen worden opgeslagen in AEM Assets. Eigenschappen van elementen bevatten de metagegevens van een bestand op een afzonderlijk tabblad voor metagegevens van elementen
* Licentieverwijzingen toevoegen aan metagegevens van elementen

### Profiel van activa

* Een gebruiker kan een Adobe Stock-profiel selecteren onder *Gebruiker > Mijn Voorkeuren > Stock Configuration*
* Aan het venster Asset Licensing kunnen verplichte en optionele verwijzingen worden toegevoegd.
* Mogelijkheid om taalvoorkeur te kiezen voor het venster Asset Licensing op basis van de regio.

### Filter

* Een gebruiker kan voorraadelementen filteren op basis van Elementtype, Oriëntatie en Vergelijkbare weergave
* Elementtype omvat Foto&#39;s, Illustraties, Vectoren, Video&#39;s, Sjablonen, 3D, Premium, Redactioneel
* De richting omvat Horizontaal, Verticaal en Vierkant.
* Voor een vergelijkbaar filter is Adobe Stock-bestandsnummer vereist

### Toegangsbeheer

* Beheerders kunnen bepaalde gebruikers/groepen machtigingen verlenen om tijdens het instellen van de Adobe Stock-configuratie voor cloudservices een licentie voor stockmiddelen te maken.
* Als een specifieke gebruiker/groep geen toestemming heeft om een licentie te verlenen voor voorraadactiva, *Zoeken naar aandelen/licenties van activa* de capaciteit zou worden uitgeschakeld .

## Adobe Stock instellen met AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 biedt gebruikers de mogelijkheid om Adobe Stock-middelen rechtstreeks vanuit AEM te zoeken, voor te vertonen, op te slaan en in licentie te geven. In deze video wordt uitgelegd hoe u met behulp van Adobe I/O Console Adobe-stops kunt instellen met AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/25043?quality=12&learn=on)

>[!NOTE]
>
>Voor de Adobe Stock Cloud-serviceconfiguratie moet u de productieomgeving en het pad naar middelen met licentie selecteren naar `/content/dam`. Het veld Omgeving wordt nu verwijderd in AEM.

>[!NOTE]
>
>De integratie vereist een [Enterprise Adobe Stock-abonnement](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) en AEM 6.4 met ten minste [Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulation&amp;fix=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc &amp;layout=list&amp;p.offset=0&amp;p.limit=24) geïmplementeerd. Zie voor AEM 6.4 service pack details [releaseopmerkingen](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html). U hebt ook beheerdersmachtigingen nodig voor [Adobe I/O-console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) en Adobe Experience Manager om de integratie op te zetten.

### Installatie {#installations}

* Voor AEM 6.4 moet u de [AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulation&amp;fix=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc &amp;layout=list&amp;p.offset=0&amp;p.limit=24) en installeer vervolgens het ZIP-bestand cq-dam-stock-integration-content-1.0.4.zip opnieuw.
* Zorg ervoor dat u beheerdersmachtigingen hebt op [Adobe I/O-console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) en Adobe Experience Manager om de integratie op te zetten.

#### Adobe IMS-configuratie instellen met Adobe I/O Console {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Een Adobe IMS Technical Account Configuration maken onder **Gereedschappen > Beveiliging**
2. Selecteer de *Cloudoplossing* als *Adobe Stock* en maak een nieuw certificaat of hergebruik een bestaand certificaat voor de configuratie.
3. Navigeer naar de Adobe I/O Console en maak een nieuwe integratie van de Rekening van de Dienst voor *Adobe Stock*.
4. Upload het certificaat van Stap2 naar uw integratie van de Adobe Stock Service Account.
5. Kies de vereiste Adobe Stock-profielconfiguratie en voltooi de service-integratie.
6. Gebruik de integratiegegevens om de configuratie van de technische account van Adobe IMS te voltooien
7. Controleer of u het toegangstoken kunt ontvangen met de technische account van Adobe IMS.

![Technische account van Adobe IMS](assets/screen_shot_2018-10-22at12219pm.png)

#### Adobe Stock-Cloud Servicen instellen {#set-up-adobe-stock-cloud-services}

1. Een nieuwe cloudserviceconfiguratie maken voor Adobe Stock onder **Opties > Cloud Servicen.**
2. Selecteer de *Adobe IMS-configuratie* gemaakt in de bovenstaande sectie voor uw *Adobe Stock Cloud* configuratie

3. Zorg ervoor dat u de optie **MILIEU** als PROD.
4. **Pad met licentiegegevens** kan worden verwezen naar elke directory onder `/content/dam`.
5. Selecteer de landinstelling en voer de installatie uit.
6. U kunt ook gebruikers/groepen toevoegen aan uw Adobe Stock Cloud-service om toegang voor specifieke gebruikers of groepen in te schakelen.

![Adobe-middelenvoorraadconfiguratie](assets/screen_shot_2018-10-22at12425pm.png)

### Aanvullende bronnen

* [Enterprise Stock Plan](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Opmerkingen bij de release AEM 6.4 Service Pack 2](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html)
* [AEM en Adobe Stock integreren](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [Adobe I/O Console-integratie-API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API-documenten](https://www.adobe.io/apis/creativecloud/stock/docs.html)
