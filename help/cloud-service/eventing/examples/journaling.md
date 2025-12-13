---
title: Journaling en AEM Events
description: Leer hoe u de eerste set AEM Events ophaalt uit het tijdschrift en de details van elke gebeurtenis bekijkt.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
duration: 280
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
exl-id: 33eb0757-f0ed-4c2d-b8b9-fa6648e87640
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---

# Journaling en AEM Events

Leer hoe u de eerste set AEM Events ophaalt uit het tijdschrift en de details van elke gebeurtenis bekijkt.

>[!VIDEO](https://video.tv.adobe.com/v/3427052?quality=12&learn=on)

Journaling is een pull-methode om AEM Events te gebruiken, en een dagboek is een geordende lijst van gebeurtenissen. Met de Adobe I/O Events Journaling-API kunt u de AEM Events ophalen uit het tijdschrift en deze verwerken in uw toepassing. Met deze aanpak kunt u gebeurtenissen beheren op basis van een opgegeven ervaring en deze efficiënt bulksgewijs verwerken. Verwijs naar [&#x200B; het Journaling &#x200B;](https://developer.adobe.com/events/docs/guides/journaling_intro/) voor diepgaande inzichten, met inbegrip van essentiële overwegingen zoals bewaartermijnen, paginering, en meer.

In het Adobe Developer Console-project wordt elke gebeurtenisregistratie automatisch ingeschakeld voor journalistiek, zodat naadloze integratie mogelijk is.

>[!IMPORTANT]
>
>De levende demo eindpunten in dit leerprogramma werden eerder ontvangen op [&#x200B; Glitch &#x200B;](https://glitch.com/). Vanaf juli 2025 heeft Glitch zijn hostingservice stopgezet en zijn de eindpunten niet langer toegankelijk.
>We werken actief aan het migreren van demo&#39;s naar een alternatief platform. De inhoud van de zelfstudie blijft accuraat en de bijgewerkte koppelingen worden binnenkort weergegeven.
>Dank u voor uw begrip en geduld.

Gebruik uw eigen toepassing tot de live demo-eindpunten weer beschikbaar zijn.

## Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

- Het milieu van AEM as a Cloud Service met [&#x200B; toegelaten de Gebeurtenis van AEM &#x200B;](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [&#x200B; Adobe Developer Console project dat voor de Gebeurtenissen van AEM &#x200B;](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console) wordt gevormd.

## Toegang tot webtoepassing

Voer de volgende stappen uit om toegang te krijgen tot de door Adobe verschafte webtoepassing:

- Verifieer u tot [&#x200B; Glitch kunt toegang hebben - ontvangen Webtoepassing &#x200B;](https://indigo-speckle-antler.glitch.me/) in een nieuwe browser tabel.

  ![&#x200B; Glitch - ontvangen Webtoepassing &#x200B;](../assets/examples/journaling/glitch-hosted-web-application.png)

## Adobe Developer Console-projectgegevens verzamelen

Om de Gebeurtenissen van AEM van het dagboek te halen, geloofsbrieven zoals _identiteitskaart van de Organisatie IMS_, _identiteitskaart van de Cliënt_, en _Token van de Toegang_ worden vereist. Voer de volgende stappen uit om deze gegevens te verzamelen:

- In [&#x200B; Adobe Developer Console &#x200B;](https://developer.adobe.com), navigeer aan uw project en klik om het te openen.

- Onder de **sectie van Referenties**, klik de **Server-aan-Server** verbinding van OAuth om de **details van Credentials** tabel te openen.

- Klik **produceren toegangstoken** knoop om het toegangstoken te produceren.

  ![&#x200B; Adobe Developer Console Project produceert Token van de Toegang &#x200B;](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- Kopieer het **Gegenereerde toegangstoken**, **identiteitskaart van de CLIENT**, en **identiteitskaart van de ORGANISATIE**. U hebt ze later nodig in deze zelfstudie.

  ![&#x200B; Referenties van het Exemplaar van het Project van Adobe Developer Console &#x200B;](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- Elke gebeurtenisregistratie wordt automatisch ingeschakeld voor journalistiek. Om het _unieke het journaling API eindpunt_ van uw gebeurtenisregistratie te krijgen, klik de gebeurteniskaart die aan de Gebeurtenissen van AEM wordt ingetekend. Van het **lusje van de Details van de Registratie**, kopieer **JOURNALING UNIQUE API EINDPUNT**.

  ![&#x200B; de Kaart van de Gebeurtenissen van het Project van Adobe Developer Console &#x200B;](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## AEM Events-journaal laden

Om de zaken eenvoudig te houden, haalt deze gehoste Webtoepassing slechts de eerste partij van de Gebeurtenissen van AEM uit het dagboek. Dit zijn de oudste beschikbare gebeurtenissen in het dagboek. Voor meer details, zie [&#x200B; eerste partij van gebeurtenissen &#x200B;](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- In het [&#x200B; Glitch - ontvangen Webtoepassing &#x200B;](https://indigo-speckle-antler.glitch.me/), ga **identiteitskaart van de Organisatie IMS**, **identiteitskaart van de Cliënt** in, en **Token van de Toegang** u vroeger van het project van Adobe Developer Console kopieerde en **klikt voorlegt**.

- Als dit lukt, worden de gegevens in het AEM Events Journal weergegeven in de tabelcomponent.

  ![&#x200B; Gegevens van het Dagboek van de Gebeurtenissen van AEM &#x200B;](../assets/examples/journaling/load-journal.png)

- Dubbelklik op de rij om de volledige gebeurtenislading weer te geven. U ziet dat de AEM-gebeurtenisdetails alle benodigde informatie hebben om de gebeurtenis in de webhaak te verwerken. Bijvoorbeeld het gebeurtenistype (`type`), gebeurtenisbron (`source`), gebeurtenis id (`event_id`), gebeurtenistijd (`time`) en gebeurtenisgegevens (`data`).

  ![&#x200B; Volledige Payload van de Gebeurtenis van AEM &#x200B;](../assets/examples/journaling/complete-journal-data.png)

## Aanvullende bronnen

- [&#x200B; Adobe I/O Events het Journaling API &#x200B;](https://developer.adobe.com/events/docs/guides/api/journaling_api/) verstrekt gedetailleerde informatie over API als eerste, volgende, en laatste partij gebeurtenissen, paginering, en meer.
