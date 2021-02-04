---
title: Verifiëren voor AEM als Cloud Service van een externe toepassing
description: Onderzoek hoe een externe toepassing programmatically met AEM als Cloud Service over HTTP kan voor authentiek verklaren en in wisselwerking staan gebruikend de Tokens van de Toegang van de Lokale Ontwikkeling en de Credentials van de Dienst.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
translation-type: tm+mt
source-git-commit: 733382dc0e0ca14d4bd6e49174ba33f8d7fc517d
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 0%

---


# Token-gebaseerde authentificatie aan AEM als Cloud Service

In dit leerprogramma onderzoekt goed hoe een externe toepassing programmatically met als Cloud Service over HTTP kan voor authentiek verklaren en in wisselwerking staan gebruikend toegangstokens.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

## Voorwaarden

Zorg ervoor dat het volgende is geïnstalleerd voordat u deze zelfstudie gaat volgen:

1. Toegang tot AEM als Cloud Service-omgeving (bij voorkeur een ontwikkelomgeving of een Sandbox-programma)
1. Lidmaatschap in de AEM als de Diensten van de Auteur van de Cloud Service AEM het Profiel van het Product van de Beheerder
1. Lidmaatschap in, of toegang tot, uw Adobe IMS Org Beheerder (zij zullen een eenmalig initialisering van [de Referenties van de Dienst](./service-credentials.md) moeten uitvoeren)
1. De nieuwste [WKND-site](https://github.com/adobe/aem-guides-wknd) geïmplementeerd in uw Cloud Service-omgeving

## Overzicht van externe toepassing

Deze zelfstudie gebruikt een [eenvoudige Node.js-toepassing](./assets/aem-guides_token-authentication-external-application.zip) die vanaf de opdrachtregel wordt uitgevoerd om metagegevens van elementen op AEM bij te werken als een Cloud Service met behulp van [Elementen HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

De uitvoeringsstroom van de toepassing Node.js is als volgt:

![Externe toepassing](./assets/overview/external-application.png)

1. De toepassing Node.js wordt aangeroepen vanaf de opdrachtregel
1. Parameters voor opdrachtregel definiëren:
   + De AEM als de dienstgastheer van de Auteur van de Cloud Service om met (`aem`) te verbinden
   + De map met AEM elementen waarvan de elementen worden bijgewerkt (`folder`)
   + De eigenschap en waarde van metagegevens die moeten worden bijgewerkt (`propertyName` en `propertyValue`)
   + Het lokale pad naar het bestand met de vereiste gegevens voor toegang tot AEM als Cloud Service (`file`)
1. Het toegangstoken dat wordt gebruikt om aan AEM voor authentiek te verklaren wordt afgeleid uit het JSON- dossier dat via bevellijnparameter `file` wordt verstrekt

   a. Als Service Credentials wordt gebruikt voor niet-lokale ontwikkeling in het JSON-bestand (`file`) wordt opgegeven, wordt het toegangstoken opgehaald uit Adobe IMS API&#39;s
1. De toepassing gebruikt het toegangstoken om tot AEM toegang te hebben en van alle activa in de omslag een lijst te maken die in de bevellijnparameter `folder` wordt gespecificeerd
1. Voor elk element in de map werkt de toepassing de metagegevens bij op basis van de naam en de waarde van de eigenschap die zijn opgegeven in de opdrachtregelparameters `propertyName` en `propertyValue`

Hoewel deze voorbeeldtoepassing Node.js is, kunnen deze interacties worden ontwikkeld gebruikend verschillende programmeertalen en uit andere externe systemen worden uitgevoerd.

## Toegangstoken lokale ontwikkeling

De Tokens van de Toegang van de Lokale Ontwikkeling worden geproduceerd voor een specifieke AEM als milieu van de Cloud Service en het verlenen van toegang tot de auteur en de Publish diensten.  Deze toegangstokens zijn tijdelijk, en moeten slechts tijdens de ontwikkeling van externe toepassingen of systemen worden gebruikt die met AEM over HTTP in wisselwerking staan. In plaats van dat een ontwikkelaar bonafide Service Credentials moet verkrijgen en beheren, kunnen ze snel en gemakkelijk zelf een tijdelijk toegangstoken genereren waarmee ze hun integratie kunnen ontwikkelen.

+ [Hoe te om het Token van de Toegang van de Lokale Ontwikkeling te gebruiken](./local-development-access-token.md)

## Servicereferenties

De geloofsbrieven van de dienst zijn de bonafide geloofsbrieven die in om het even welke niet ontwikkelingsscenario&#39;s - het duidelijkst productie - worden gebruikt die een externe toepassing of de capaciteit van het systeem om aan voor authentiek te verklaren en met, als Cloud Service over HTTP in wisselwerking te staan AEM vergemakkelijken. De geloofsbrieven van de dienst zelf worden niet verzonden naar AEM voor authentificatie, in plaats daarvan gebruikt de externe toepassing deze om JWT te produceren, die met APIs _for_ van Adobe IMS een toegangstoken wordt geruild, die dan kan worden gebruikt om HTTP- verzoeken om als Cloud Service te AEM voor authentiek te verklaren.

+ [Hoe te om de Referenties van de Dienst te gebruiken](./service-credentials.md)

## Aanvullende bronnen

+ [De voorbeeldtoepassing downloaden](./assets/aem-guides_token-authentication-external-application.zip)
+ Andere codevoorbeelden van JWT-creatie en -uitwisseling
   + [Node.js, Java, Python, C#.NET en PHP codesteekproeven](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Codevoorbeeld op basis van JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
