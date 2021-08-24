---
title: Een site maken
seo-title: Aan de slag met AEM Sites - Een site maken
description: Gebruik de wizard Site maken in Adobe Experience Manager AEM om een nieuwe website te genereren. Het standaardSjabloon voor site dat door Adobe wordt verschaft, wordt gebruikt als beginpunt voor de nieuwe site.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Inhoudsbeheer
feature: Kerncomponenten, pagina-editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---


# Een site maken {#create-site}

>[!CAUTION]
>
> De functies voor snel maken van sites die hier worden getoond, worden in de tweede helft van 2021 gepubliceerd. De bijbehorende documentatie is beschikbaar voor voorbeelddoeleinden.

In dit hoofdstuk wordt ingegaan op het maken van een nieuwe site in Adobe Experience Manager. Het standaardSjabloon voor de site, opgegeven door Adobe, wordt gebruikt als beginpunt.

## Vereisten {#prerequisites}

De stappen in dit hoofdstuk vinden plaats in een Adobe Experience Manager als een Cloud Service-omgeving. Zorg ervoor dat u beheertoegang hebt tot de AEM. Het wordt aanbevolen een [Sandbox-programma](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) en [Development environment](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) te gebruiken bij het voltooien van deze zelfstudie.

Raadpleeg de [on boarding documentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) voor meer informatie.

## Doelstelling {#objective}

1. Leer hoe u de wizard Site maken kunt gebruiken om een nieuwe site te maken.
1. Begrijp de rol van de Malplaatjes van de Plaats.
1. Ontdek de gegenereerde AEM.

## Aanmelden bij Adobe Experience Manager-auteur {#author}

Als eerste stap, login aan uw AEM als milieu van de Cloud Service. AEM omgevingen worden gesplitst tussen een **Auteur-service** en een **Publish-service**.

* **Auteursservice**  - waar site-inhoud wordt gemaakt, beheerd en bijgewerkt. Doorgaans hebben alleen interne gebruikers toegang tot de **Auteursservice** en bevindt deze zich achter een aanmeldingsscherm.
* **Service**  voor publiceren: host de live website. Dit is de dienst die de eindgebruikers zullen zien en typisch openbaar beschikbaar zijn.

Het grootste deel van de zelfstudie wordt uitgevoerd met de **Auteursservice**.

1. Navigeer naar de Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Meld u aan met uw persoonlijke account of een bedrijf-/schoolaccount.
1. Zorg ervoor dat de juiste organisatie is geselecteerd in het menu en klik op **Experience Manager**.

   ![Experience Cloud Home](assets/create-site/experience-cloud-home-screen.png)

1. Klik onder **Cloud Manager** op **Launch**.
1. Houd de muis boven het programma dat u wilt gebruiken en klik op het pictogram **Cloud Manager Program**.

   ![programmapictogram van Cloud Manager](assets/create-site/cloud-manager-program-icon.png)

1. Klik in het bovenste menu op **Omgevingen** om de geleverde omgevingen weer te geven.

1. Zoek de omgeving die u wilt gebruiken en klik op **Auteur-URL**.

   ![Access dev-auteur](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Het wordt aanbevolen een **Development**-omgeving te gebruiken voor deze zelfstudie.

1. Er wordt een nieuw tabblad gestart voor de AEM **Auteursservice**. Klik **Teken binnen met Adobe** en u zou automatisch met de zelfde geloofsbrieven van de Experience Cloud moeten worden het programma geopend.

1. Nadat u bent omgeleid en geverifieerd, kunt u nu het AEM startscherm bekijken.

   ![Startscherm AEM](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Heb je problemen met de toegang tot Experience Manager? Raadpleeg de [on boarding documentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## De basissjabloon voor de site downloaden

Een Sjabloon voor site biedt een beginpunt voor een nieuwe site. Een Sjabloon van een site bevat enkele basisthema&#39;s, paginasjablonen, configuraties en voorbeeldinhoud. Precies wat in het Sjabloon van de Site is opgenomen, is aan de ontwikkelaar. Adobe biedt een **Basic Site Template** om nieuwe implementaties te versnellen.

1. Open een nieuw browser lusje en navigeer aan het Basis project van het Malplaatje van de Plaats op GitHub: [https://github.com/adobe/aem-site-template-basic](https://github.com/adobe/aem-site-template-basic). Het project is open-sourced en heeft een licentie om door iedereen te worden gebruikt.
1. Klik **Geeft** vrij en navigeer naar [recentste versie](https://github.com/adobe/aem-site-template-basic/releases/latest).
1. Vouw het vervolgkeuzemenu **Middelen** uit en download het ZIP-bestand van de sjabloon:

   ![Standaardsitemaldo](assets/create-site/template-basic-zip-file.png)

   Dit ZIP-bestand wordt gebruikt in de volgende oefening.

   >[!NOTE]
   >
   > Deze zelfstudie wordt geschreven met behulp van versie **5.0.0** van de Basic Site Template. Als u een nieuw project start, wordt u altijd aangeraden de nieuwste versie te gebruiken.

## Een nieuwe site maken

Daarna, produceer een nieuwe plaats gebruikend het Malplaatje van de Plaats van de vorige oefening.

1. Ga terug naar de AEM. Navigeer van het AEM scherm van het Begin aan **Plaatsen**.
1. Klik in de rechterbovenhoek op **Maken** > **Site (Sjabloon)**. Hiermee wordt de wizard **Site maken** weergegeven.
1. Klik onder **Selecteer een sitesjabloon** op de knop **Importeren**.

   Upload het **.zip** malplaatjedossier dat van de vorige oefening wordt gedownload.

1. Selecteer **Basic AEM Site Template** en klik **Next**.

   ![Sitesjabloon selecteren](assets/create-site/select-site-template.png)

1. Onder **Site-details** > **Site-titel** voert u `WKND Site` in.
1. Typ `wknd` onder **Sitenaam**.

   ![Sitesjabloondetails](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Als u een gedeelde AEM gebruikt, voegt u een unieke id toe aan **Sitenaam**. Bijvoorbeeld `wknd-johndoe`. Dit zal ervoor zorgen dat de veelvoudige gebruikers het zelfde leerprogramma, zonder enige botsingen kunnen voltooien.

1. Klik **Maken** om de site te genereren. Klik **Done** in het **Success** dialoogvenster wanneer AEM klaar is met het maken van de website.

## De nieuwe site verkennen

1. Navigeer naar de AEM Sites-console, als deze nog niet aanwezig is.
1. Er is een nieuwe **WKND-site** gegenereerd. Dit omvat een sitestructuur met een meertalige hiërarchie.
1. Open de pagina **English** > **Home** door de pagina te selecteren en op de knop **Edit** in de menubalk te klikken:

   ![WKND-sitehiërarchie](assets/create-site/wknd-site-starter-hierarchy.png)

1. Starter-inhoud is al gemaakt en er zijn verschillende componenten beschikbaar die aan een pagina moeten worden toegevoegd. Experimenteer met deze componenten om een idee te krijgen van de functionaliteit. In het volgende hoofdstuk leert u de grondbeginselen van een component.

   ![Startpagina starten](assets/create-site/start-home-page.png)

   *Voorbeeldinhoud van het Sitesjabloon*

## Gefeliciteerd! {#congratulations}

U hebt zojuist uw eerste AEM-site gemaakt.

### Volgende stappen {#next-steps}

Gebruik de pagina-editor in Adobe Experience Manager AEM om de inhoud van de site in het hoofdstuk [Author-inhoud bij te werken en te publiceren](author-content-publish.md). Leer hoe atoomcomponenten kunnen worden gevormd om inhoud bij te werken. Begrijp het verschil tussen een auteur AEM en publiceer milieu&#39;s en leer hoe te om updates aan de levende plaats te publiceren.
