---
title: Verifiëren voor AEM as a Cloud Service vanuit een externe toepassing
description: Onderzoek hoe een externe toepassing programmatically met AEM as a Cloud Service over HTTP kan voor authentiek verklaren en in wisselwerking staan gebruikend de Tokens van de Toegang van de Lokale Ontwikkeling en de Referenties van de Dienst.
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 253
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 0%

---

# Tokengebaseerde verificatie naar AEM as a Cloud Service

AEM stelt verschillende HTTP-eindpunten bloot waarmee zonder kop kan worden gewerkt, van GraphQL, AEM Content Services tot de Assets HTTP API. Deze gebruikers zonder kop moeten zich vaak verifiëren bij AEM om toegang te krijgen tot beveiligde inhoud of handelingen. AEM ondersteunt tokengebaseerde verificatie van HTTP-aanvragen van externe toepassingen, services of systemen om dit te vergemakkelijken.

In deze zelfstudie leert u goed hoe een externe toepassing via programmacode AEM as a Cloud Service kan verifiëren en ermee kan communiceren via HTTP. Hiervoor wordt gebruikgemaakt van toegangstokens.

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## Voorwaarden

Zorg ervoor dat het volgende is geïnstalleerd voordat u deze zelfstudie gaat volgen:

1. Toegang tot de omgeving van am AEM as a Cloud Service (bij voorkeur een ontwikkelomgeving of een Sandbox-programma)
1. Lidmaatschap in AEM Administrator Product Profile van de AEM as a Cloud Service-omgeving
1. Lidmaatschap in, of toegang, tot uw Beheerder van IMS van Adobe (zij zullen een eenmalige initialisering van de [ Referenties van de Dienst ](./service-credentials.md) moeten uitvoeren)
1. De recentste [ plaats WKND ](https://github.com/adobe/aem-guides-wknd) die aan uw milieu van Cloud Service wordt opgesteld

## Overzicht van externe toepassing

Dit leerprogramma gebruikt a [ eenvoudige toepassing Node.js ](./assets/aem-guides_token-authentication-external-application.zip) looppas van de bevellijn om activa meta-gegevens op AEM as a Cloud Service bij te werken gebruikend [ HTTP API van Assets ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

De uitvoeringsstroom van de toepassing Node.js is als volgt:

![ Externe Toepassing ](./assets/overview/external-application.png)

1. De toepassing Node.js wordt aangeroepen vanaf de opdrachtregel
1. Parameters voor opdrachtregel definiëren:
   + De AEM as a Cloud Service Author service host to connect to (`aem`)
   + De map met AEM-middelen waarvan de elementen zijn bijgewerkt (`folder`)
   + De eigenschap en waarde van metagegevens die moeten worden bijgewerkt (`propertyName` en `propertyValue`)
   + Het lokale pad naar het bestand met de vereiste gegevens voor toegang tot AEM as a Cloud Service (`file`)
1. Het toegangstoken dat wordt gebruikt voor verificatie naar AEM, is afgeleid van het JSON-bestand dat via opdrachtregelparameter wordt aangeboden `file`

   a. Als Service Credentials gebruikt voor niet-lokale ontwikkeling wordt opgegeven in het JSON-bestand (`file`), wordt het toegangstoken opgehaald van Adobe IMS API&#39;s
1. De toepassing gebruikt het toegangstoken om AEM te openen en alle elementen in de map weer te geven die in de opdrachtregelparameter is opgegeven `folder`
1. Voor elk element in de map werkt de toepassing de metagegevens bij op basis van de naam en de waarde van de eigenschap die zijn opgegeven in de opdrachtregelparameters `propertyName` en `propertyValue`

Hoewel deze voorbeeldtoepassing Node.js is, kunnen deze interacties worden ontwikkeld gebruikend verschillende programmeertalen en uit andere externe systemen worden uitgevoerd.

## Toegangstoken lokale ontwikkeling

De Tokens van de Toegang van de Lokale Ontwikkeling worden geproduceerd voor een specifieke milieu van AEM as a Cloud Service en het verlenen van toegang tot de auteur en de Publish diensten.  Deze toegangstokens zijn tijdelijk en mogen alleen worden gebruikt tijdens de ontwikkeling van externe toepassingen of systemen die via HTTP werken met AEM. In plaats van dat een ontwikkelaar bonafide Service Credentials moet verkrijgen en beheren, kunnen ze snel en gemakkelijk zelf een tijdelijk toegangstoken genereren waarmee ze hun integratie kunnen ontwikkelen.

+ [Hoe te om het Token van de Toegang van de Lokale Ontwikkeling te gebruiken](./local-development-access-token.md)

## Servicereferenties

De geloofsbrieven van de dienst zijn de bonafide geloofsbrieven die in om het even welke niet ontwikkelingsscenario&#39;s - het duidelijkst productie - worden gebruikt die een externe toepassing of de capaciteit van het systeem om aan voor authentiek te verklaren en met AEM as a Cloud Service over HTTP in wisselwerking te staan vergemakkelijken. De Referenties van de dienst zelf worden niet verzonden naar AEM voor authentificatie, in plaats daarvan gebruikt de externe toepassing deze om JWT te produceren, die met APIs van Adobe IMS _voor_ een toegangstoken wordt geruild, dat dan kan worden gebruikt om HTTP- verzoeken aan AEM as a Cloud Service voor authentiek te verklaren.

+ [Hoe te om de Referenties van de Dienst te gebruiken](./service-credentials.md)

## Aanvullende bronnen

+ [De voorbeeldtoepassing downloaden](./assets/aem-guides_token-authentication-external-application.zip)
+ Andere codevoorbeelden van JWT maken en uitwisselen
   + [ Node.js, Java, Python, C#.NET, en PHP codesteekproeven ](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [ JavaScript/Axios-Gebaseerde codesteekproef ](https://github.com/adobe/aemcs-api-client-lib)
