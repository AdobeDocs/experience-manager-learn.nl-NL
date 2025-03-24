---
title: Een AEM UI-extensie verifiëren
description: Leer hoe u een AEM UI-extensie kunt voorvertonen, testen en verifiëren voordat u deze implementeert in productie.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603, KT-13382
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
duration: 600
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '739'
ht-degree: 0%

---

# Een extensie verifiëren

AEM UI-extensies kunnen worden gecontroleerd op elke AEM as a Cloud Service-omgeving in de Adobe Org waartoe de extensie behoort.

Het testen van een extensie gebeurt via een speciaal gemaakte URL die AEM de instructie geeft de extensie alleen voor die aanvraag te laden.

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

>[!IMPORTANT]
>
> In de bovenstaande video ziet u het gebruik van een extensie van Content Fragment Console om de voorvertoning en verificatie van de App Builder-extensie-app te illustreren. Nochtans, is het belangrijk om op te merken dat de behandelde concepten op alle uitbreidingen van de gebruikersinterface van AEM kunnen worden toegepast.

## URL AEM-interface

![ URL van de Console van het Fragment van AEM Inhoud ](./assets/verify/content-fragment-console-url.png){align="center"}

Als u een URL wilt maken die de extensie non-production in AEM koppelt, moet de URL van de AEM-interface waarin de extensie wordt geïnjecteerd, worden verkregen. Navigeer naar de AEM as a Cloud Service-omgeving om de extensie te controleren en open de gebruikersinterface waarin de extensie moet worden voorvertoond.

Als u bijvoorbeeld een extensie wilt voorvertonen voor de Content Fragment-console:

1. Meld u aan bij de gewenste AEM as a Cloud Service-versie.
1. Selecteer het __pictogram van de Fragmenten van de Inhoud__.
1. Wacht tot de AEM Content Fragment Console in de browser is geladen.
1. Kopieer de URL van de AEM Content Fragment Console van de adresbalk van de browser en het lijkt erop:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Deze URL wordt hieronder gebruikt bij het maken van de URL&#39;s voor ontwikkeling en werkgebiedverificatie. Als u de extensie verifieert aan de hand van andere gebruikersinterface van AEM, vraagt u deze URL&#39;s op en past u onderstaande dezelfde stappen toe.

## Builds voor lokale ontwikkeling controleren

1. Open een opdrachtregel naar de hoofdmap van het extensieproject.
1. De AEM UI-extensie uitvoeren als een lokale App Builder-app

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

Noteer de URL van de lokale toepassing, die hierboven wordt weergegeven als `-> https://localhost:9080`

1. Aanvankelijk (en wanneer u een Fout van de Verbinding ziet) open `https://localhost:9080` (of wat uw lokale toepassings URL) in uw Webbrowser is, en keurt manueel [ het HTTPS certificaat ](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) goed.
1. Voeg de volgende twee vraagparams aan [ toe AEM UI URL ](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>` , meestal `&ext=https://localhost:9080` .

   Voeg de twee bovengenoemde vraagparameters (`devMode` en `ext`) als __eerste__ vraagparameters in URL toe. AEM die verlengbare UI gebruikshash routes (`#/@wknd/aem/...`), zo verkeerd post-bevestigend de parameters nadat `#` niet werkt.

   De voorbeeld-URL moet er als volgt uitzien:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Kopieer en plak de URL van de voorvertoning in uw browser.

   + U kunt moeten aanvankelijk, en dan periodiek, [ het certificaat HTTPS ](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) voor de gastheer van de lokale toepassing (`https://localhost:9080`) goedkeuren.

1. De gebruikersinterface van AEM wordt geladen met de lokale versie van de extensie die in de interface is geïnjecteerd voor verificatie.

>[!IMPORTANT]
>
>Houd er rekening mee dat bij het gebruik van deze benadering de extensie die wordt ontwikkeld alleen van invloed is op uw ervaring. Alle andere gebruikers van de gebruikersinterface van AEM ervaren de gebruikersinterface zonder de geïnjecteerde extensie.

## Werkgebiedbuilds verifiëren

1. Open een opdrachtregel naar de hoofdmap van het extensieproject.
1. Controleer of de werkruimte van het werkgebied actief is (of welke Workspace ook wordt gebruikt voor verificatie).

   ```shell
   $ aio app use -w Stage
   ```

   Voeg eventuele wijzigingen in `.env` en `.aio` samen.

1. Implementeer de bijgewerkte extensie App Builder-app. Als u zich niet hebt aangemeld, voert u `aio login` eerst uit.

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

1. Voeg de volgende twee vraagparams aan [ toe AEM UI URL ](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Voeg de twee bovengenoemde vraagparameters (`devMode` en `ext`) als __eerste__ vraagparameters in URL toe, aangezien verlengbare AEM UIs een knoeiboelroute (`#/@wknd/aem/...`) gebruikt, zo verkeerd post-bevestigend de parameters nadat `#` niet werkt.

   De voorbeeld-URL moet er als volgt uitzien:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Kopieer en plak de URL van de voorvertoning in uw browser.
1. Met de AEM Content Fragment Console wordt de versie van de extensie geïnjecteerd naar de werkruimte van het werkgebied. Deze werkgebied-URL kan ter verificatie worden gedeeld met een kwaliteitscontrole of zakelijke gebruikers.

Wanneer u deze methode gebruikt, wordt de extensie Staged alleen geïnjecteerd in AEM Content Fragment Console wanneer u toegang hebt tot de URL van het werkgebied.

1. Geïmporteerde extensies kunnen worden bijgewerkt door `aio app deploy` opnieuw uit te voeren. Deze wijzigingen worden automatisch doorgevoerd wanneer u de URL van de voorvertoning gebruikt.
1. Voer `aio app undeploy` uit om een extensie voor verificatie te verwijderen.

## Voorvertoning bladwijzer

Om het maken van URL&#39;s met voorvertoningen en voorvertoningen zoals hierboven beschreven, te vergemakkelijken, kunt u een JavaScript-bladwijzer maken die de extensie laadt.

De referentie hieronder previews [ lokale ontwikkeling bouwt ](#verify-local-development-builds) van de uitbreiding op `https://localhost:9080`. Aan voorproef [ bouwt het stadium ](#verify-stage-builds), creeer een bookmarklet met de `previewApp` variabele die aan URL van opgestelde toepassing van App Builder wordt geplaatst.

1. Maak een bladwijzer in uw browser.
1. Bewerk de bladwijzer.
1. Geef een bladwijzer een betekenisvolle naam, zoals `AEM UI Extension Preview (localhost:9080)`.
1. Stel de URL van de bladwijzer in op de volgende code:

   ```javascript
   javascript: (() => {
       /* Change this to the URL of the local App Builder app if not using https://localhost:9080 */
       const previewApp = 'https://localhost:9080';
   
       const repo = new URL(window.location.href).searchParams.get('repo');
   
       if (window.location.href.match(/https:\/\/experience\.adobe\.com\/.*\/aem\/cf\/(editor|admin)\/.*/i)) {
           window.location = `https://experience.adobe.com/?devMode=true&ext=${previewApp}&repo=${repo}${window.location.hash}`;
       } 
   })();
   ```

1. Navigeer naar een uitbreidbare AEM UI om de voorvertoningsextensie te laden en klik vervolgens op de bladwijzer.

>[!TIP]
>
> Als de App Builder-extensie niet wordt geladen en u gebruikt `&ext=https://localhost:9080`, opent u die host en poort rechtstreeks op een browsertabblad en accepteert u het zelfondertekende certificaat. Probeer de bladwijzer opnieuw.
