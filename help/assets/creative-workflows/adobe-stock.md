---
title: Adobe Stock-middelen gebruiken met AEM Assets
description: AEM biedt gebruikers de mogelijkheid om Adobe Stock-middelen rechtstreeks vanuit AEM te zoeken, voor te vertonen, op te slaan en in licentie te geven. Organisaties kunnen hun Adobe Stock Enterprise-plan nu integreren met AEM Assets om ervoor te zorgen dat gelicentieerde middelen nu ruim beschikbaar zijn voor hun creatieve en marketingprojecten, met de krachtige mogelijkheden voor middelenbeheer van AEM.
feature: Adobe Stock
version: Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
doc-type: Feature Video
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
duration: 1079
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 0%

---

# Adobe Stock gebruiken met AEM Assets{#using-adobe-stock-assets-with-aem-assets}

Met AEM 6.4.2 kunnen gebruikers Adobe Stock-middelen rechtstreeks vanuit AEM zoeken, voorvertonen, opslaan en in licentie geven. Organisaties kunnen hun Adobe Stock Enterprise-plan nu integreren met AEM Assets om ervoor te zorgen dat gelicentieerde middelen nu ruim beschikbaar zijn voor hun creatieve en marketingprojecten, met de krachtige mogelijkheden voor middelenbeheer van AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678?quality=12&learn=on)

>[!NOTE]
>
>De integratie vereist een [&#x200B; ondernemingsAdobe Stock plan &#x200B;](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) en AEM 6.4 met minstens opgesteld Service Pack 2. Voor AEM 6.4 de details van het de dienstpak, zie deze [&#x200B; versienota&#39;s &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-4/release-notes/sp-release-notes.html).

Dankzij de Adobe Stock- en AEM Assets-integratie kunnen auteurs en marketers van content eenvoudig een licentie voor en gebruik van stock assets maken voor creatieve of marketingdoeleinden. U kunt een zoekopdracht voor Stock-elementen uitvoeren met Omni Search, door het locatiefilter toe te voegen als Adobe Stock of door door de belangrijkste navigatie van AEM Assets te navigeren en op het UI-pictogram Zoeken in Adobe Stock Coral te klikken.

## Mogelijkheden

### Zoeken en opslaan

* Adobe Stock-middelen zoeken zonder de AEM-werkruimte te verlaten.
* Sla Adobe Stock-middelen op voor een voorvertoning, zonder dat u een licentie voor het element hoeft aan te schaffen.
* Mogelijkheid om Adobe Stock-middelen in licentie te geven en op te slaan naar AEM Assets
* Mogelijkheid om vergelijkbare middelen te zoeken vanuit Adobe Stock in de gebruikersinterface van AEM Assets
* Een geselecteerd middel van Stock Search in AEM Assets bekijken op de Adobe Stock-website
* Bestanden met licentiebronnen zijn gemarkeerd met een blauwe, licentie-badge voor eenvoudige identificatie

### Metagegevens van element

* In licentie gegeven middelen worden opgeslagen in AEM Assets. Eigenschappen van elementen bevatten de metagegevens van een bestand op een afzonderlijk tabblad voor metagegevens van elementen
* Licentieverwijzingen toevoegen aan metagegevens van elementen

### Profiel van activa

* Een gebruiker kan het profiel van Adobe Stock onder *Gebruiker selecteren > Mijn Voorkeur > de Configuratie van de Beeld*
* Aan het venster Asset Licensing kunnen verplichte en optionele verwijzingen worden toegevoegd.
* Mogelijkheid om taalvoorkeur te kiezen voor het venster Asset Licensing op basis van de regio.

### Filter

* Een gebruiker kan voorraadelementen filteren op basis van Elementtype, Oriëntatie en Vergelijkbare weergave
* Elementtype omvat Foto&#39;s, Illustraties, Vectoren, Video&#39;s, Sjablonen, 3D, Premium, Redactioneel
* De richting omvat Horizontaal, Verticaal en Vierkant.
* Voor een vergelijkbaar filter is Adobe Stock-bestandsnummer vereist

### Toegangsbeheer

* Beheerders kunnen bepaalde gebruikers/groepen machtigingen verlenen om tijdens het instellen van de Adobe Stock-configuratie voor cloudservices een licentie voor stockmiddelen te maken.
* Als een specifieke gebruiker/een groep geen toestemming heeft om voorraadactiva in licentie te geven, *het Onderzoek van het Activa van het Middel/het verlenen van vergunningen van Activa* vermogen zou worden onbruikbaar gemaakt.

## Adobe Stock instellen met AEM Assets{#set-up-adobe-stock-with-aem-assets}

Met AEM 6.4.2 kunnen gebruikers Adobe Stock-middelen rechtstreeks vanuit AEM zoeken, voorvertonen, opslaan en in licentie geven. In deze video wordt uitgelegd hoe u Adobe-bestanden met AEM Assets kunt instellen met Adobe I/O Console.

>[!VIDEO](https://video.tv.adobe.com/v/25043?quality=12&learn=on)

>[!NOTE]
>
>Voor de Adobe Stock Cloud-serviceconfiguratie moet u de productieomgeving selecteren en het middelenpad met licentie wijzen naar `/content/dam` . Het veld Environment is nu verwijderd in AEM.

>[!NOTE]
>
>De integratie vereist een [&#x200B; ondernemingsAdobe Stock plan &#x200B;](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) en AEM 6.4 met minstens [&#x200B; Geïmplementeerde Service Pack 2 &#x200B;](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulation&amp;fix=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc &amp;layout=list&amp;p.offset=0&amp;p.limit=24). Voor AEM 6.4 de details van het de dienstpak, zie deze [&#x200B; versienota&#39;s &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-4/release-notes/sp-release-notes.html). U zou ook beheerdertoestemmingen aan [&#x200B; Console van Adobe I/O &#x200B;](https://console.adobe.io/), [&#x200B; Adobe Admin Console &#x200B;](https://adminconsole.adobe.com/) en Adobe Experience Manager aan opstelling de integratie nodig hebben.

### Installatie {#installations}

* Voor AEM 6.4, moet u [&#x200B; AEM Service Pack 2 &#x200B;](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulation&amp;fix=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc &amp;layout=list&amp;p.offset=0&amp;p.limit=24) installeren en dan het cq-dam-stock-integration-content-1.0.4.zip- dossier opnieuw installeren.
* Zorg ervoor u beheerdertoestemmingen op [&#x200B; de Console van Adobe I/O &#x200B;](https://console.adobe.io/), [&#x200B; Adobe Admin Console &#x200B;](https://adminconsole.adobe.com/) en Adobe Experience Manager aan opstelling de integratie hebt.

#### Adobe IMS-configuratie instellen met Adobe I/O Console {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Creeer een Configuratie van de Rekening van Adobe IMS Technische onder **Hulpmiddelen > Veiligheid**
2. Selecteer de *Oplossing van de Wolk* als *Adobe Stock* en creeer een nieuw certificaat of hergebruik een bestaand certificaat voor de configuratie.
3. Navigeer aan de Console van Adobe I/O en creeer een nieuwe integratie van de Rekening van de Dienst voor *Adobe Stock*.
4. Upload het certificaat van Stap2 naar uw integratie van de Adobe Stock Service Account.
5. Kies de vereiste Adobe Stock-profielconfiguratie en voltooi de service-integratie.
6. Gebruik de integratiegegevens om de configuratie van de technische account van Adobe IMS te voltooien
7. Controleer of u het toegangstoken kunt ontvangen met de technische account van Adobe IMS.

![&#x200B; de Technische Rekening van Adobe IMS &#x200B;](assets/screen_shot_2018-10-22at12219pm.png)

#### Adobe Stock Cloud Services instellen {#set-up-adobe-stock-cloud-services}

1. Creeer een nieuwe configuratie van de wolkendienst voor Adobe Stock onder **Hulpmiddelen > de Diensten van de Wolk.**
2. Selecteer de *Configuratie van Adobe IMS* die in de bovengenoemde sectie voor uw *wordt gecreeerd Adobe Stock Cloud* configuratie

3. Zorg ervoor u het **MILIEU** als PROD selecteert.
4. **Vergunning gegeven weg van Activa** kan aan om het even welke folder onder `/content/dam` worden gericht.
5. Selecteer de landinstelling en voer de installatie uit.
6. U kunt ook gebruikers/groepen toevoegen aan uw Adobe Stock Cloud-service om toegang voor specifieke gebruikers of groepen in te schakelen.

![&#x200B; Adobe Assets Stock Configuration &#x200B;](assets/screen_shot_2018-10-22at12425pm.png)

### Aanvullende bronnen

* [&#x200B; Plan van de Voorraad van de Onderneming &#x200B;](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [&#x200B; AEM 6.4 Service Pack 2 de nota&#39;s van de Versie &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html?lang=nl-NL)
* [&#x200B; integreer AEM en Adobe Stock &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html?lang=nl-NL)
* [&#x200B; de Integratie API van de Console van Adobe I/O &#x200B;](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [&#x200B; Adobe Stock API Docs &#x200B;](https://www.adobe.io/apis/creativecloud/stock/docs.html)
