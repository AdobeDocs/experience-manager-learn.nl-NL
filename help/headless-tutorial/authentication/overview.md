---
title: Verifiëren voor AEM as a Cloud Service vanuit een externe toepassing
description: Onderzoek hoe een externe toepassing programmatically met AEM as a Cloud Service over HTTP kan voor authentiek verklaren en in wisselwerking staan gebruikend de Tokens van de Toegang van de Lokale Ontwikkeling en de Referenties van de Dienst.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---

# Token-gebaseerde authentificatie aan AEM as a Cloud Service

AEM stelt een verscheidenheid van eindpunten van HTTP bloot die met op een krantenloze manier, van GraphQL, AEM de Diensten van de Inhoud aan de API van Activa kunnen worden in wisselwerking staan. Vaak moeten deze gebruikers zonder kop zich op AEM verifiëren om toegang te krijgen tot beveiligde inhoud of handelingen. Om dit te vergemakkelijken, steunt AEM symbolisch-gebaseerde authentificatie van HTTP- verzoeken van externe toepassingen, diensten of systemen.

In deze zelfstudie leert u goed hoe een externe toepassing programmatically kan verifiëren en met AEM as a Cloud Service over HTTP in wisselwerking staan gebruikend toegangstokens.

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## Voorwaarden

Zorg ervoor dat het volgende is geïnstalleerd voordat u deze zelfstudie gaat volgen:

1. Toegang tot AEM as a Cloud Service omgeving (bij voorkeur een ontwikkelomgeving of een Sandbox-programma)
1. Lidmaatschap in de AEM as a Cloud Service omgeving: Auteursservices AEM Beheerdersproductprofiel
1. Lidmaatschap in of toegang tot uw Adobe IMS Org Administrator (deze moet een eenmalige initialisatie van de [Servicereferenties](./service-credentials.md))
1. De nieuwste [WKND-site](https://github.com/adobe/aem-guides-wknd) geïmplementeerd in uw Cloud Service-omgeving

## Overzicht van externe toepassing

Deze zelfstudie gebruikt een [simple Node.js, toepassing](./assets/aem-guides_token-authentication-external-application.zip) Voer vanaf de opdrachtregel uit om metagegevens van elementen bij te werken bij AEM as a Cloud Service met behulp van [Elementen HTTP-API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

De uitvoeringsstroom van de toepassing Node.js is als volgt:

![Externe toepassing](./assets/overview/external-application.png)

1. De toepassing Node.js wordt aangeroepen vanaf de opdrachtregel
1. Parameters voor opdrachtregel definiëren:
   + De AEM as a Cloud Service host van de auteurservice waarmee verbinding moet worden gemaakt (`aem`)
   + De map met AEM elementen waarvan de elementen zijn bijgewerkt (`folder`)
   + De eigenschap en waarde van de metagegevens die moeten worden bijgewerkt (`propertyName` en `propertyValue`)
   + Het lokale pad naar het bestand met de vereiste gegevens voor toegang tot AEM as a Cloud Service (`file`)
1. Het toegangstoken dat wordt gebruikt om voor authentiek te verklaren aan AEM wordt afgeleid uit het JSON- dossier dat via bevellijnparameter wordt verstrekt `file`

   a. Als de Referenties van de Dienst die voor niet lokale ontwikkeling worden gebruikt in het JSON dossier worden verstrekt (`file`), wordt het toegangstoken opgehaald van Adobe IMS API&#39;s
1. De toepassing gebruikt het toegangstoken om tot AEM toegang te hebben en van alle activa in de omslag een lijst te maken die in de parameter van de bevellijn wordt gespecificeerd `folder`
1. Voor elk element in de map werkt de toepassing de metagegevens bij op basis van de naam en de waarde van de eigenschap die zijn opgegeven in de opdrachtregelparameters. `propertyName` en `propertyValue`

Hoewel deze voorbeeldtoepassing Node.js is, kunnen deze interacties worden ontwikkeld gebruikend verschillende programmeertalen en uit andere externe systemen worden uitgevoerd.

## Toegangstoken lokale ontwikkeling

De Tokens van de Toegang van de Lokale Ontwikkeling worden geproduceerd voor een specifieke AEM as a Cloud Service milieu en het verlenen van toegang tot de auteur en de Publish diensten.  Deze toegangstokens zijn tijdelijk, en moeten slechts tijdens de ontwikkeling van externe toepassingen of systemen worden gebruikt die met AEM over HTTP in wisselwerking staan. In plaats van dat een ontwikkelaar bonafide Service Credentials moet verkrijgen en beheren, kunnen ze snel en gemakkelijk zelf een tijdelijk toegangstoken genereren waarmee ze hun integratie kunnen ontwikkelen.

+ [Hoe te om het Token van de Toegang van de Lokale Ontwikkeling te gebruiken](./local-development-access-token.md)

## Servicereferenties

De geloofsbrieven van de dienst zijn de bonafide geloofsbrieven die in om het even welke niet ontwikkelingsscenario&#39;s - het duidelijkst productie - worden gebruikt die een externe toepassing of de capaciteit van het systeem vergemakkelijken om aan voor authentiek te verklaren en met, AEM as a Cloud Service over HTTP in wisselwerking te staan. De servicereferenties zelf worden niet verzonden naar AEM voor verificatie, maar de externe toepassing gebruikt deze om een JWT te genereren, die wordt uitgewisseld met de API&#39;s van Adobe IMS _for_ een toegangstoken, dat dan kan worden gebruikt om de verzoeken van HTTP voor AEM as a Cloud Service voor authentiek te verklaren.

+ [Hoe te om de Referenties van de Dienst te gebruiken](./service-credentials.md)

## Aanvullende bronnen

+ [De voorbeeldtoepassing downloaden](./assets/aem-guides_token-authentication-external-application.zip)
+ Andere codevoorbeelden van JWT-creatie en -uitwisseling
   + [Node.js, Java, Python, C#.NET en PHP codesteekproeven](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [Codevoorbeeld op basis van JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
