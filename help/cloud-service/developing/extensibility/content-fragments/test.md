---
title: Een extensie voor AEM Content Fragment-console testen
description: Leer hoe te om een AEM de consoleuitbreiding van het Fragment van de Inhoud te testen alvorens aan productie op te stellen.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: 56e2cbadaceb9961de28454bfbed56a98df34c44
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Een extensie testen

AEM de uitbreidingen van de Console van het Fragment van de Inhoud kunnen tegen om het even welke AEM as a Cloud Service milieu in de Adobe worden getest die de uitbreiding tot behoort.

Het testen van een uitbreiding wordt gedaan door een speciaal gemaakte URL die de AEM console van het Fragment van de Inhoud opdraagt om de uitbreiding te laden.

## URL van AEM Content Fragment Console

![URL van AEM Content Fragment Console](./assets/test/content-fragment-console-url.png){align="center"}

Als u een URL wilt maken waarmee de extensie niet-Productie wordt gekoppeld aan een AEM Content Fragment Console, moet de URL van de gewenste AEM Content Fragment Console worden verzameld. Navigeer naar de AEM as a Cloud Service omgeving om de extensie te testen en kopieer de URL van de AEM Content Fragment Console.

1. Meld u aan bij de gewenste AEM as a Cloud Service omwenteling.

   + Gebruik een ontwikkelomgeving voor AEM [testontwikkelingsbuilds](#testing-development-builds)
   + De ontwikkelomgeving voor AEM of QA gebruiken voor [constructies van testfasen](#testing-stage-builds)

1. Selecteer __Inhoudsfragmenten__ pictogram.
1. Wacht tot de AEM Content Fragment Console in de browser is geladen.
1. Kopieer de URL van de AEM Content Fragment Console van de adresbalk van de browser en het lijkt erop:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Deze URL wordt hieronder gebruikt bij het maken van de URL&#39;s voor ontwikkeling en het testen van het werkgebied.

## Bouwwerken voor lokale ontwikkeling testen

1. Open een opdrachtregel naar de hoofdmap van het extensieproject.
1. De extensie AEM Content Fragment Console uitvoeren als een lokale App Builder-app

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

Let op de URL van de lokale toepassing, die hierboven wordt weergegeven als `-> https://localhost:9080`

1. Voeg de volgende twee vraagparams aan toe [URL van AEM Content Fragment Console](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, gewoonlijk `&ext=https://localhost:9080`.

   Voeg de twee bovenstaande queryparameters toe (`devMode` en `ext`) als de __first__ query-parameters in de URL, aangezien de Content Fragment Console een hash-route gebruikt (`#/@wknd/aem/...`), zodat de parameters na de `#` werkt niet.

   De test-URL moet er als volgt uitzien:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Kopieer en plak de test-URL in uw browser.

   + Het kan nodig zijn om eerst en dan periodiek [het HTTPS-certificaat accepteren](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) voor de host van de lokale toepassing (`https://localhost:9080`).

1. De AEM Content Fragment Console wordt geladen met de lokale versie van de extensie die erin is geïnjecteerd voor testdoeleinden, en wijzigingen die tijdens het hot-reload worden toegepast zolang de lokale App Builder-app wordt uitgevoerd.

>[!IMPORTANT]
>
>Herinner me, wanneer het gebruiken van deze benadering, de uitbreiding in ontwikkeling slechts uw ervaring beïnvloedt, en alle andere gebruikers van de AEM console van het Fragment van de Inhoud hebben toegang tot het zonder de geïnjecteerde uitbreiding.


## Constructies van testfasen

1. Open een opdrachtregel naar de hoofdmap van het extensieproject.
1. Controleer of de werkruimte van het werkgebied actief is (of welke werkruimte dan ook wordt gebruikt voor het testen).

   ```shell
   $ aio app use -w Stage
   ```
   Alle wijzigingen samenvoegen in `.env` en `.aio`.
1. Implementeer de bijgewerkte extensie App Builder-app. Als u zich niet hebt aangemeld, uitvoeren `aio login` eerst.

   ```shell
   $ aio app deploy
   ...
   Your deployed actions:
   web actions:
     -> https://98765-123aquarat.adobeio-static.net/api/v1/web/aem-cf-console-admin-1/generic 
   To view your deployed application:
     -> https://98765-123aquarat.adobeio-static.net/index.html
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://98765-123aquarat.adobeio-static.net/index.html
   New Extension Point(s) in Workspace 'Production': 'aem/cf-console-admin/1'
   Successful deployment 🏄
   ```

1. Voeg de volgende twee vraagparams aan toe [URL van AEM Content Fragment Console](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Voeg de twee bovenstaande queryparameters toe (`devMode` en `ext`) als de __first__ query-parameters in de URL, aangezien de Content Fragment Console een hash-route gebruikt (`#/@wknd/aem/...`), zodat de parameters na de `#` werkt niet.

   De test-URL moet er als volgt uitzien:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Kopieer en plak de test-URL in uw browser.
1. Met de AEM Content Fragment Console wordt de versie van de extensie geïnjecteerd die is geïmplementeerd in de werkruimte van het werkgebied. Deze werkgebied-URL kan voor testdoeleinden worden gedeeld met een kwaliteitscontrole of zakelijke gebruikers.

Houd er rekening mee dat wanneer u deze methode gebruikt, de extensie Staged alleen wordt geïnjecteerd op AEM Content Fragment Console&#39;s bij toegang tot de URL van het werkgebied van het vaartuig.

1. Ingevoerde extensies kunnen worden bijgewerkt door `aio app deploy` en deze wijzigingen worden automatisch doorgevoerd wanneer u de test-URL gebruikt.
1. Als u een extensie voor testen wilt verwijderen, voert u `aio app undeploy`.



