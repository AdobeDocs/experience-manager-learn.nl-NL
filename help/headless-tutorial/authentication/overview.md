---
title: Verifiëren voor AEM as a Cloud Service vanuit een externe toepassing
description: Onderzoek hoe een externe toepassing programmatically met AEM as a Cloud Service over HTTP kan voor authentiek verklaren en in wisselwerking staan gebruikend de Tokens van de Toegang van de Lokale Ontwikkeling en de Referenties van de Dienst.
version: Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 253
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 0%

---

# Tokengebaseerde verificatie naar AEM as a Cloud Service

AEM stelt een verscheidenheid van eindpunten van HTTP bloot die met op een krantenloze manier, van GraphQL, AEM de Diensten van de Inhoud aan Assets HTTP API kunnen worden in wisselwerking staan. Vaak moeten deze gebruikers zonder kop zich op AEM verifiëren om toegang te krijgen tot beveiligde inhoud of handelingen. Om dit te vergemakkelijken, steunt AEM symbolisch-gebaseerde authentificatie van HTTP- verzoeken van externe toepassingen, diensten of systemen.

In deze zelfstudie leert u goed hoe een externe toepassing via programmacode AEM as a Cloud Service kan verifiëren en ermee kan communiceren via HTTP. Hiervoor wordt gebruikgemaakt van toegangstokens.

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## Voorwaarden

Zorg ervoor dat het volgende is geïnstalleerd voordat u deze zelfstudie gaat volgen:

1. Toegang tot de omgeving van am AEM as a Cloud Service (bij voorkeur een ontwikkelomgeving of een Sandbox-programma)
1. Lidmaatschap in de AEM as a Cloud Service-services voor auteurs AEM Beheerdersproductprofiel
1. Lidmaatschap in, of toegang, tot uw Beheerder van IMS van Adobe (zij zullen een eenmalige initialisering van de [ Referenties van de Dienst ](./service-credentials.md) moeten uitvoeren)
1. De recentste [ Plaats van WKND ](https://github.com/adobe/aem-guides-wknd) die aan uw milieu van de Cloud Service wordt opgesteld

## Overzicht van externe toepassing

Dit leerprogramma gebruikt a [ eenvoudige toepassing Node.js ](./assets/aem-guides_token-authentication-external-application.zip) looppas van de bevellijn om activa meta-gegevens op AEM as a Cloud Service bij te werken gebruikend [ HTTP API van Assets ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

De uitvoeringsstroom van de toepassing Node.js is als volgt:

![ Externe Toepassing ](./assets/overview/external-application.png)

1. De toepassing Node.js wordt aangeroepen vanaf de opdrachtregel
1. Parameters voor opdrachtregel definiëren:
   + De AEM as a Cloud Service Author service host to connect to (`aem`)
   + De map met AEM elementen waarvan de elementen zijn bijgewerkt (`folder`)
   + De eigenschap en waarde van metagegevens die moeten worden bijgewerkt (`propertyName` en `propertyValue`)
   + Het lokale pad naar het bestand met de vereiste gegevens voor toegang tot AEM as a Cloud Service (`file`)
1. Het toegangstoken dat wordt gebruikt voor verificatie aan AEM wordt afgeleid van het JSON-bestand dat via opdrachtregelparameter wordt aangeboden `file`

   a. Als Service Credentials gebruikt voor niet-lokale ontwikkeling wordt opgegeven in het JSON-bestand (`file`), wordt het toegangstoken opgehaald van Adobe IMS API&#39;s
1. De toepassing gebruikt het toegangstoken om toegang te krijgen tot AEM en alle elementen weer te geven in de map die is opgegeven in de opdrachtregelparameter `folder`
1. Voor elk element in de map werkt de toepassing de metagegevens bij op basis van de naam en de waarde van de eigenschap die zijn opgegeven in de opdrachtregelparameters `propertyName` en `propertyValue`

Hoewel deze voorbeeldtoepassing Node.js is, kunnen deze interacties worden ontwikkeld gebruikend verschillende programmeertalen en uit andere externe systemen worden uitgevoerd.

## Toegangstoken lokale ontwikkeling

De Tokens van de Toegang van de Lokale Ontwikkeling worden geproduceerd voor een specifieke milieu van AEM as a Cloud Service en het verlenen van toegang tot de diensten van de Auteur en van Publish.  Deze toegangstokens zijn tijdelijk, en moeten slechts tijdens de ontwikkeling van externe toepassingen of systemen worden gebruikt die met AEM over HTTP in wisselwerking staan. In plaats van dat een ontwikkelaar bonafide Service Credentials moet verkrijgen en beheren, kunnen ze snel en gemakkelijk zelf een tijdelijk toegangstoken genereren waarmee ze hun integratie kunnen ontwikkelen.

+ [Hoe te om het Token van de Toegang van de Lokale Ontwikkeling te gebruiken](./local-development-access-token.md)

## Servicereferenties

De geloofsbrieven van de dienst zijn de bonafide geloofsbrieven die in om het even welke niet ontwikkelingsscenario&#39;s - het duidelijkst productie - worden gebruikt die een externe toepassing of de capaciteit van het systeem om aan voor authentiek te verklaren en met AEM as a Cloud Service over HTTP in wisselwerking te staan vergemakkelijken. De geloofsbrieven van de dienst zelf worden niet verzonden naar AEM voor authentificatie, in plaats daarvan gebruikt de externe toepassing deze om JWT te produceren, die met APIs van Adobe IMS _voor_ een toegangstoken wordt geruild, dat dan kan worden gebruikt om HTTP- verzoeken aan AEM as a Cloud Service voor authentiek te verklaren.

+ [Hoe te om de Referenties van de Dienst te gebruiken](./service-credentials.md)

## Aanvullende bronnen

+ [De voorbeeldtoepassing downloaden](./assets/aem-guides_token-authentication-external-application.zip)
+ Andere codevoorbeelden van JWT maken en uitwisselen
   + [ Node.js, Java, Python, C#.NET, en PHP codesteekproeven ](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [ JavaScript/Axios-Gebaseerde codesteekproef ](https://github.com/adobe/aemcs-api-client-lib)
