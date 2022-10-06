---
title: Adobe Asset Link en AEM
description: Adobe Experience Manager-middelen kunnen door ontwerpers en creatieve gebruikers worden gebruikt in hun favoriete Adobe Creative Cloud-bureaubladtoepassingen. Adobe Asset Link-extensie voor Adobe Creative Cloud for enterprise breidt de mogelijkheid uit om metagegevens van AEM elementen in Creative Cloud-gereedschappen, zoals Adobe XD, Photoshop, InDesign en Illustrator, te doorzoeken, te sorteren, voor te vertonen, te uploaden, uit te checken, te wijzigen, in te checken en weer te geven.
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
exl-id: 6c49f8c2-f468-4b29-b7b6-029c8ab39ce9
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# Adobe Asset Link 3.0

Adobe Experience Manager-middelen kunnen door ontwerpers en creatieve gebruikers worden gebruikt in hun favoriete Adobe Creative Cloud-bureaubladtoepassingen.

Adobe Asset Link-extensie voor Adobe Creative Cloud for enterprise breidt de mogelijkheid uit om metagegevens van AEM elementen in Creative Cloud-toepassingen te zoeken en te zoeken, te sorteren, voor te vertonen, te uploaden, te checken, te wijzigen, in te checken en weer te geven.

>[!TIP]
>
> Meer informatie over hoe [Adobe XD Premium-trainingsprogramma](https://spark.adobe.com/page/wU7OXv8qKGugO/) kan u helpen Asset Link te integreren met uw Adobe Experience Manager-workflow.

## Adobe Asset Link en AEM creatieve workflows

De volgende video illustreert een algemene workflow die wordt gebruikt door ontwerpers die in Adobe Creative Cloud-toepassingen werken en die rechtstreeks met AEM met behulp van Adobe Asset Link integreren.

>[!VIDEO](https://video.tv.adobe.com/v/335927/?quality=12&learn=on)

## Mogelijkheden voor Adobe-itemkoppeling

+ Adobe Asset Link kan worden geïntegreerd met AEM Assets en Assets Essentials.
+ Adobe Asset Link configureert automatisch verbinding met op cloud gebaseerde AEM (AEM Assets as a Cloud Service en Assets Essentials)
+ Adobe Asset Link is een extensie die werkt in Adobe Creative Cloud-toepassingen:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Automatische verificatie voor AEM met hun Adobe Enterprise ID of Federated ID
+ Zoeken naar digitale middelen in AEM
+ De dossierdetails van de toegang voor activa verblijven in AEM van met het paneel:
   + Miniatuur
   + Standaardmetagegevens
   + Versies
+ Elementen plaatsen, downloaden of slepen en neerzetten in hun layout
+ Elementen wijzigen door ze uit te checken van AEM en er aan te werken (WIP) binnen hun Creative Cloud Assets-account
+ Activeer een element weer in AEM nadat ze klaar zijn met het wijzigen ervan en de nieuwe versie wordt weerspiegeld in AEM
+ Middelen zoeken in AEM vanuit het deelvenster Koppeling in app voor Adobe-element
+ Bladeren door AEM Assets-verzamelingen en slimme verzamelingen rechtstreeks vanuit het deelvenster Asset Link
+ Nieuw gemaakte elementen rechtstreeks vanuit het deelvenster AEM toevoegen
+ Elementen rechtstreeks naar InDesign-frames slepen en neerzetten

## Elementen in InDesign plaatsen

Adobe Asset Link biedt ondersteuning voor InDesign direct koppelen tussen Adobe Asset Link en AEM. Met ondersteuning voor rechtstreekse koppelingen naar InDesign kunt u plaatsen (__Gekoppelde plaatsen__ of __Kopie plaatsen__) of sleep digitale elementen van AEM naar InDesign via het deelvenster Adobe Asset Link. Introduceert ook de uitvoering *For Placement Only+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Gebruik alleen de Adobe Creative Cloud-Enterprise ID of -Federated ID. Zorg ervoor dat u [configureren van AEM voor Adobe Asset Link](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

U kunt een element op een van de volgende manieren in de lay-out InDesign plaatsen:

+ **Kopie plaatsen** - Als u een element insluit (met de optie Kopie plaatsen), wordt een kopie van het oorspronkelijke element in de lay-out InDesign geplaatst nadat de binaire bestanden naar uw lokale systeem zijn gedownload. Adobe Asset Link behoudt geen koppeling tussen de ingesloten kopie en het oorspronkelijke element. Als het oorspronkelijke element is gewijzigd in AEM, moet u het ingesloten element verwijderen uit het InDesign-bestand en het element opnieuw insluiten uit AEM.

+ **Gekoppelde plaatsen** - Wanneer u met InDesign-documenten werkt, kunt u naar de elementen van AEM verwijzen en de elementen rechtstreeks insluiten (met de optie Kopie plaatsen in het contextmenu). Door naar elementen te verwijzen, kunt u samenwerken met andere gebruikers en eventuele updates van het oorspronkelijke element in AEM opnemen. Als u vanuit AEM wilt verwijzen naar een element, gebruikt u de optie Gekoppelde plaatsen in het contextmenu.

### Alleen voor plaatsingsafbeeldingen

Wanneer grote elementbestanden vanuit AEM Adobe Asset Link in InDesign-documenten worden geplaatst, moeten creatieve gebruikers enkele seconden wachten nadat ze de plaatsingsbewerking hebben gestart. Dit beïnvloedt de algemene gebruikerservaring. Met Adobe Asset Link kunt u tijdelijk een afbeelding met een lage resolutie van het oorspronkelijke element uit AEM plaatsen, waardoor de tijd die nodig is om een afbeelding te plaatsen, korter wordt. Tegelijkertijd vergroot het de algemene gebruikerservaring en productiviteit. De afbeelding met een lagere resolutie wordt tijdelijk geplaatst en wanneer de uiteindelijke uitvoer vereist is voor afdrukken of publiceren, moet u de FPO-uitvoeringen vervangen door de originelen. Als u meerdere FPO-afbeeldingen wilt vervangen door de respectievelijke originele afbeeldingen, navigeert u naar **_Windows > Koppelingen_** en downloadt u de originele elementen. Nadat de oorspronkelijke afbeeldingen zijn gedownload, kiest u Alle FPO&#39;s vervangen door originelen.

FPO-uitvoeringen zijn lichte vervangingen van de oorspronkelijke activa. Ze hebben dezelfde hoogte-breedteverhouding, maar ze zijn kleiner dan de oorspronkelijke afbeeldingen. InDesign ondersteunt momenteel alleen het importeren van FPO-uitvoeringen voor de volgende afbeeldingstypen:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Als een FPO-uitvoering niet beschikbaar is voor een specifiek middel in AEM, wordt in plaats daarvan verwezen naar het oorspronkelijke middel met hoge resolutie. Voor FPO-afbeeldingen wordt de status FPO weergegeven in het deelvenster Koppelingen InDesign.

## Adobe Asset Link-verificatie met AEM Assets

Hoe Adobe Asset Link-verificatie werkt in de context van Adobe Identity Management Services (IMS) en Adobe Experience Manager Author.

![Adobe Asset Link Architecture](assets/adobe-asset-link-article-understand.png)

1. De Adobe Asset Link-extensie vraagt via de Adobe Creative Cloud Desktop App om toestemming voor Adobe Identity Manage Service (IMS) en ontvangt bij succes een token voor de gebruiker.
1. Adobe Asset Link-extensie maakt verbinding met AEM-auteur via HTTP(S), inclusief de token voor Drager die wordt verkregen in **Stap 1** met gebruik van het schema (HTTP/HTTPS), host en poort die in de instellingen van de extensie JSON zijn opgegeven.
1. AEM de handler voor identiteitsverificatie aan toonder extraheert de token Beonder uit de aanvraag en valideert deze met Adobe IMS.
1. Nadat Adobe IMS de token heeft gevalideerd, wordt een gebruiker in AEM gemaakt (als deze nog niet bestaat) en worden profiel- en groep- en lidmaatschapsgegevens van Adobe IMS gesynchroniseerd. De AEM gebruiker krijgt een standaard AEM aanmeldingstoken, dat wordt teruggestuurd naar de extensie Adobe Asset Link als cookie in de HTTP(S)-reactie.
1. Volgende interacties (d.w.z. zoeken, zoeken, in- en uitchecken, enz.) met de extensie Adobe Asset Link resulteert in HTTP(S)-aanvragen bij AEM-auteur die worden gevalideerd met het aanmeldingstoken AEM, met de standaard AEM Token Authentication Handler.

>[!NOTE]
>
>Na afloop van het aanmeldingstoken **Stap 1-5** zal automatisch aanhalen, voor authentiek verklaart de uitbreiding van de Verbinding van het Middel van Adobe gebruikend het teken van de Drager, en geeft een nieuw, geldig login teken opnieuw uit.

## Aanvullende bronnen

+ [Adobe Asset Link-website](https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html)
