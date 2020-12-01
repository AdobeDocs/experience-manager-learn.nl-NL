---
title: Adobe Asset Link Extension gebruiken met AEM Assets
description: 'Adobe Experience Manager-middelen kunnen nu door ontwerpers en creatieve gebruikers worden gebruikt in hun favoriete Adobe Creative Cloud-bureaubladtoepassingen. Adobe Asset Link-extensie voor Adobe Creative Cloud Enterprise breidt de mogelijkheid uit om metagegevens van AEM middelen in Creative Cloud-gereedschappen, zoals Adobe Photoshop, InDesign en Illustrator, te zoeken en te zoeken, te sorteren, voor te vertonen, te uploaden, uit te checken, te wijzigen, in te checken en weer te geven. '
feature: adobe-asset-link
topics: authoring, collaboration, operations, sharing, metadata, images
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1092'
ht-degree: 0%

---


# Adobe Asset Link Extension gebruiken met AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}

Adobe Experience Manager-middelen kunnen nu door ontwerpers en creatieve gebruikers worden gebruikt in hun favoriete Adobe Creative Cloud-bureaubladtoepassingen. Adobe Asset Link-extensie voor Adobe Creative Cloud Enterprise breidt de mogelijkheid uit om metagegevens van AEM middelen in Creative Cloud-gereedschappen, zoals Adobe Photoshop, InDesign en Illustrator, te zoeken en te zoeken, te sorteren, voor te vertonen, te uploaden, uit te checken, te wijzigen, in te checken en weer te geven.


## Adobe Asset Link 1.1

Adobe Asset Link v1.1 biedt nu ondersteuning voor InDesign direct koppelen tussen Adobe Asset Link en AEM Assets. Met de ondersteuning voor rechtstreekse koppelingen voor InDesign kunt u nu via het deelvenster Koppeling voor Adobe-middelen digitale elementen plaatsen (Gekoppelde plaatsen of Kopie plaatsen) of naar InDesign slepen vanuit AEM Assets. Introduceert ook de *Alleen voor plaatsing* (FPO)-uitvoering.

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Gebruik alleen de Adobe Creative Cloud-Enterprise ID of -Federated ID. Zorg ervoor u [vorm AEM voor de Verbinding van Activa van Adobe ](https://helpx.adobe.com/enterprise/using/configure-aem-for-aal-prerelease.html).


### Adobe Asset Link Capabilities

* Adobe Asset Link is een extensie die werkt binnen PS, AI, ID en directe toegang biedt tot digitale middelen die in AEM Assets zijn opgeslagen
* Creatieve personen worden automatisch AEM aangemeld met hun Adobe IMS-Enterprise ID of -Federated ID.
* Creatieve personen kunnen niet alleen zoeken in AEM Assets en Creative Cloud Assets, maar ook door digitale middelen in AEM Assets bladeren
* Creatieve personen hebben toegang tot de bestandsgegevens van in AEM Assets wonende middelen; miniatuur, basismetagegevens en versies in het deelvenster
* Klanten kunnen elementen in hun lay-out plaatsen, downloaden of slepen
* Creatieve personen kunnen elementen wijzigen door ze vanuit AEM Assets af te checken en er aan te werken (WIP) binnen hun Creative Cloud Assets-account
* Creatieve personen kunnen een middel terugcontroleren naar AEM Assets nadat ze het hebben gewijzigd. De nieuwe versie wordt weerspiegeld in AEM Assets
* Ondersteunt Creative Cloud 2020-, 2019- en 2018-InDesign-, Photoshop- en Illustrator-bureaubladapps
* Een gebruiker kan een elementenonderzoek van het paneel van de Verbinding van de Activa van de Adobe in-app uitvoeren en hen sorteren die op grootte, alfabetisch en door relevantie worden gebaseerd
* Gebruikers kunnen AEM Assets-verzamelingen en slimme verzamelingen rechtstreeks openen en bekijken via het deelvenster Asset Link
* Nieuw gemaakte elementen rechtstreeks vanuit het deelvenster aan AEM Assets toevoegen
* Een gebruiker kan elementen rechtstreeks naar InDesign-frames slepen en neerzetten

### AEM Assets in InDesign plaatsen

U kunt een element op een van de volgende manieren in de lay-out InDesign plaatsen:

* **Kopie**  plaatsen - Een element insluiten (met de optie Kopie plaatsen) plaatst een kopie van het oorspronkelijke element in de lay-out InDesign nadat de binaire bestanden naar uw lokale systeem zijn gedownload. Adobe Asset Link behoudt geen koppeling tussen de ingesloten kopie en het oorspronkelijke element. Als het oorspronkelijke element is gewijzigd in AEM Assets, moet u het ingesloten element verwijderen uit het InDesign-bestand en het element opnieuw insluiten vanuit AEM Assets.

* **Linked**  plaatsen - Wanneer u met documenten van de InDesign werkt, hebt u nu de optie om naar de activa van AEM Assets te verwijzen naast direct het inbedden van de activa (gebruikend de optie van het Exemplaar van de Plaats in het contextmenu). Door naar elementen te verwijzen, kunt u samenwerken met andere gebruikers en eventuele updates van het oorspronkelijke element in AEM Assets opnemen. Als u vanuit AEM Assets naar een element wilt verwijzen, gebruikt u de optie Gekoppelde plaatsen in het contextmenu.

### Alleen voor FPO-resolutie (Placement Only)

Wanneer grote elementbestanden vanuit AEM Assets in InDesign-documenten worden geplaatst met behulp van Adobe Asset Link, moeten creatieve gebruikers enkele seconden wachten nadat ze de plaatsingsbewerking hebben gestart. Dit beïnvloedt de algemene gebruikerservaring. Met Adobe Asset Link kunt u nu tijdelijk een afbeelding met een lage resolutie van het oorspronkelijke element uit AEM Assets plaatsen, waardoor de tijd die nodig is om een afbeelding te plaatsen, korter wordt. Tegelijkertijd vergroot het de algemene gebruikerservaring en productiviteit. De afbeelding met een lagere resolutie wordt tijdelijk geplaatst en wanneer de uiteindelijke uitvoer vereist is voor afdrukken of publiceren, moet u de FPO-uitvoeringen vervangen door de originelen. Als u meerdere FPO-afbeeldingen wilt vervangen door de respectievelijke originele afbeeldingen, navigeert u naar **_Venster > Koppelingen_** en downloadt u de oorspronkelijke elementen. Nadat de oorspronkelijke afbeeldingen zijn gedownload, kiest u Alle FPO&#39;s vervangen door originelen.

>[!NOTE]
>
> *Voor* uitvoering alleen voor Plaatsing (FPO) werkt alleen voor de optie Gekoppeld plaatsen. U moet ook ondersteuning voor FPO-uitvoering inschakelen in de AEM Assets *Dam Update Asset*-workflow.

FPO-uitvoeringen zijn lichte vervangingen van de oorspronkelijke activa. Ze hebben dezelfde hoogte-breedteverhouding, maar ze zijn kleiner dan de oorspronkelijke afbeeldingen. InDesign ondersteunt momenteel alleen het importeren van FPO-uitvoeringen voor de volgende afbeeldingstypen:

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

Als een FPO-uitvoering niet beschikbaar is voor een specifiek element in AEM Assets, wordt in plaats daarvan verwezen naar het oorspronkelijke middel met hoge resolutie. Voor FPO-afbeeldingen wordt de status FPO weergegeven in het deelvenster Koppelingen InDesign.



## De authentificatie van de Verbinding van Adobe-activa met AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Hoe Adobe Asset Link-verificatie werkt in de context van Adobe Identity Management Services (IMS) en Adobe Experience Manager Author.

![Adobe Asset Link Architecture](assets/adobe-asset-link-article-understand.png)

[Adobe Asset Link Architecture](assets/adobe-asset-link-article-understand-1.png) downloaden

1. De Adobe Asset Link-extensie vraagt via de Adobe Creative Cloud Desktop App om toestemming voor Adobe Identity Manage Service (IMS) en ontvangt bij succes een token voor de gebruiker.
2. Adobe Asset Link-extensie maakt verbinding met AEM-auteur via HTTP(S), inclusief het token dat wordt verkregen in **Stap 1**, met behulp van het schema (HTTP/HTTPS), host en poort die in de JSON-instellingen van de extensie zijn opgegeven.
3. De manager van de Authentificatie van de AEM van de Drager haalt het token uit het verzoek en bevestigt het tegen Adobe IMS.
4. Nadat de Adobe IMS de token Betoonder heeft gevalideerd, wordt een gebruiker in AEM gemaakt (als deze nog niet bestaat) en worden de profiel- en groep-/lidmaatschapsgegevens van de Adobe IMS gesynchroniseerd. De AEM gebruiker krijgt een standaard AEM aanmeldingstoken, dat wordt teruggestuurd naar de extensie Adobe Asset Link als cookie in de HTTP(S)-reactie.
5. Volgende interacties (d.w.z. zoeken, zoeken, in- en uitchecken, enz.) met de extensie Adobe Asset Link resulteert in HTTP(S)-aanvragen bij AEM-auteur die worden gevalideerd met het aanmeldingstoken AEM, met de standaard AEM Token Authentication Handler.

>[!NOTE]
>
>Na afloop van het aanmeldingstoken wordt **Stap 1-5** automatisch aangeroepen, waarbij de extensie Adobe Asset Link wordt geverifieerd met behulp van het token Vierder en een nieuw, geldig aanmeldingstoken wordt opnieuw uitgegeven.

## Aanvullende bronnen{#additional-resources}

* [Adobe Asset Link-website](https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html)