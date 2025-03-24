---
title: Adobe Asset Link en AEM
description: Adobe Experience Manager-middelen kunnen door ontwerpers en creatieve gebruikers worden gebruikt in hun favoriete Adobe Creative Cloud-bureaubladtoepassingen. De extensie Adobe Asset Link voor Adobe Creative Cloud voor ondernemingen breidt de mogelijkheid uit om metagegevens van AEM-elementen te zoeken en te zoeken, te sorteren, voor te vertonen, te uploaden, te controleren, te wijzigen, in te checken en weer te geven in Creative Cloud-gereedschappen, zoals Adobe XD, Photoshop, InDesign en Illustrator.
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
duration: 673
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '983'
ht-degree: 0%

---


# Adobe Asset Link 3.0

Adobe Experience Manager-middelen kunnen door ontwerpers en creatieve gebruikers worden gebruikt in hun favoriete Adobe Creative Cloud-bureaubladtoepassingen.

De extensie Adobe Asset Link voor Adobe Creative Cloud voor ondernemingen breidt de mogelijkheid uit om metagegevens van AEM-elementen in Creative Cloud-toepassingen te zoeken en te zoeken, te sorteren, voor te vertonen, te uploaden, uit te checken, te wijzigen, in te checken en weer te geven.

## Adobe Asset Link-mogelijkheden

+ Adobe Asset Link kan worden geïntegreerd met AEM Assets en Assets Essentials.
+ Adobe Asset Link configureert automatisch verbinding met op cloud gebaseerde AEM-omgevingen (AEM Assets as a Cloud Service en Assets Essentials)
+ Adobe Asset Link is een extensie die werkt in Adobe Creative Cloud-toepassingen:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Automatische verificatie voor AEM met hun Adobe Enterprise ID of Federated ID
+ Zoeken naar digitale middelen in AEM
+ Via het deelvenster hebt u toegang tot bestandsgegevens voor elementen die zich in AEM bevinden:
   + Miniatuur
   + Standaardmetagegevens
   + Versies
+ Elementen plaatsen, downloaden of slepen en neerzetten in hun layout
+ Elementen wijzigen door ze van AEM af te halen en er aan te werken (WIP) binnen hun Creative Cloud Assets-account
+ Activeer een element weer in AEM nadat ze klaar zijn met het wijzigen ervan en de nieuwe versie wordt weerspiegeld in AEM
+ Middelen in AEM zoeken via het deelvenster Koppeling voor Adobe-middelen in de app
+ Bladeren door AEM Assets-verzamelingen en slimme verzamelingen rechtstreeks vanuit het deelvenster Asset Link
+ Nieuw gemaakte elementen rechtstreeks vanuit het deelvenster aan AEM toevoegen
+ Elementen rechtstreeks naar InDesign-frames slepen en neerzetten

## Elementen plaatsen in InDesign

Adobe Asset Link biedt InDesign direct linkondersteuning tussen Adobe Asset Link en AEM. Met InDesign het directe verbinden steun, kunt u (__Gekoppelde Plaats__ of __Exemplaar van de Plaats__) plaatsen of belemmering-n-daling digitale activa in InDesign van AEM via het paneel van de Verbinding van Activa van Adobe slepen. Introduceert ook de uitvoering *For Placement Only+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>Gebruik alleen Adobe Creative Cloud Enterprise ID of Federated ID. Zorg ervoor u [ AEM voor de Verbinding van Activa van Adobe ](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html) vormt.

U kunt een element op een van de volgende manieren in uw InDesign-lay-out plaatsen:

+ **het Exemplaar van de Plaats** - het Inbedden van een activa (die de optie van het Exemplaar van de Plaats gebruiken) plaatst een exemplaar van het originele element in uw lay-out van InDesign na het downloaden van de binaire bestanden aan uw lokaal systeem. Adobe Asset Link onderhoudt geen koppeling tussen de ingesloten kopie en het oorspronkelijke element. Als het oorspronkelijke element is gewijzigd in AEM, moet u het ingesloten element verwijderen uit het InDesign-bestand en het element opnieuw insluiten vanuit AEM.

+ **Gekoppelde Plaats** - wanneer het werken met de documenten van InDesign, hebt u de optie om de activa van AEM naast direct te verwijzen inbeddend de activa (gebruikend de optie van het Exemplaar van de Plaats in het contextmenu). Door naar elementen te verwijzen, kunt u samenwerken met andere gebruikers en eventuele updates van het oorspronkelijke element in AEM opnemen. Als u vanuit AEM naar een element wilt verwijzen, gebruikt u de optie Gekoppelde plaatsen in het contextmenu.

### Alleen voor plaatsingsafbeeldingen

Wanneer grote elementbestanden vanuit AEM in InDesign-documenten worden geplaatst met Adobe Asset Link, moeten creatieve gebruikers enkele seconden wachten nadat ze de plaatsingsbewerking hebben gestart. Dit beïnvloedt de algemene gebruikerservaring. Met Adobe Asset Link kunt u tijdelijk een afbeelding met een lage resolutie van het oorspronkelijke element uit AEM plaatsen, waardoor de tijd die nodig is om een afbeelding te plaatsen, korter wordt. Tegelijkertijd vergroot het de algemene gebruikerservaring en productiviteit. De afbeelding met een lagere resolutie wordt tijdelijk geplaatst en wanneer de uiteindelijke uitvoer vereist is voor afdrukken of publiceren, moet u de FPO-uitvoeringen vervangen door de originelen. Als u veelvoudige beelden FPO met respectieve originele beelden wilt vervangen, navigeer aan **_Vensters > het paneel van Verbindingen_** en download dan de originele activa. Nadat de oorspronkelijke afbeeldingen zijn gedownload, kiest u Alle FPO&#39;s vervangen door originelen.

FPO-uitvoeringen zijn lichte vervangingen van de oorspronkelijke activa. Ze hebben dezelfde hoogte-breedteverhouding, maar ze zijn kleiner dan de oorspronkelijke afbeeldingen. InDesign biedt momenteel alleen ondersteuning voor het importeren van FPO-uitvoeringen voor de volgende afbeeldingstypen:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Als een FPO-uitvoering niet beschikbaar is voor een specifiek element in AEM, wordt in plaats daarvan verwezen naar het oorspronkelijke middel met hoge resolutie. Voor FPO-afbeeldingen wordt de status FPO weergegeven in het deelvenster Koppelingen van InDesign.

## Adobe Asset Link-verificatie met AEM Assets

Hoe Adobe Asset Link-verificatie werkt in de context van Adobe Identity Management Services (IMS) en Adobe Experience Manager Author.

![ Adobe Asset Link Architecture ](assets/adobe-asset-link-article-understand.png)

1. De Adobe Asset Link-extensie vraagt via de Adobe Creative Cloud Desktop App om toestemming voor de Adobe Identity Manage Service (IMS) en ontvangt bij succes een token voor de gebruiker.
1. De uitbreiding van de Verbinding van het Activa van Adobe verbindt met de Auteur van AEM over HTTP (S), met inbegrip van het token dat van de Drager in **wordt verkregen Stap 1**, gebruikend de regeling (HTTP/HTTPS), gastheer en haven die in de montages JSON van de uitbreiding wordt verstrekt.
1. De AEM-handler voor vruchtdradenverificatie extraheert de token Betower uit de aanvraag en valideert deze met Adobe IMS.
1. Nadat Adobe IMS de token heeft gevalideerd, wordt een gebruiker gemaakt in AEM (als deze nog niet bestaat) en worden de gegevens van profielen en groepen/lidmaatschappen van Adobe IMS gesynchroniseerd. De AEM-gebruiker krijgt een standaard AEM-aanmeldingstoken, dat als cookie wordt teruggestuurd naar de Adobe Asset Link-extensie in de HTTP(S)-respons.
1. Volgende interacties (dat wil zeggen het doorbladeren, zoeken, inchecken/uitchecken van elementen, enz.) met de extensie Adobe Asset Link resulteert in HTTP(S)-aanvragen bij AEM Author die worden gevalideerd met het AEM-aanmeldingstoken, met de standaard AEM Token Authentication Handler.

>[!NOTE]
>
>Op afloop van login teken, **Stappen 1-5** automatisch zullen aanhalen, voor authentiek verklaren de uitbreiding van de Verbinding van het Activa van Adobe gebruikend het teken van de Drager, en een nieuw, geldig login teken opnieuw uitgeven.

## Aanvullende bronnen

+ [ de website van de Verbinding van Activa van Adobe ](https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html)
