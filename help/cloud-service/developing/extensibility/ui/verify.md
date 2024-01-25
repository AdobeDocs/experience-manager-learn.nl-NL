---
title: Een AEM UI-extensie verifiëren
description: Leer hoe te om, een uitbreiding van de AEM UI te voorproef te testen en te verifiëren alvorens aan productie op te stellen.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603, KT-13382
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
duration: 633
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '712'
ht-degree: 0%

---

# Een extensie verifiëren

AEM UI-extensies kunnen worden geverifieerd op basis van AEM as a Cloud Service omgeving in de Adobe of de extensie tot behoort.

Het testen van een extensie wordt uitgevoerd via een speciaal gemaakte URL die AEM de instructie geeft de extensie alleen voor die aanvraag te laden.

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

>[!IMPORTANT]
>
> In de bovenstaande video ziet u het gebruik van een extensie van Content Fragment Console om een voorbeeld en verificatie van de App Builder-extensie voor te vertonen. Nochtans, is het belangrijk om op te merken dat de behandelde concepten op alle AEM uitbreidingen kunnen worden toegepast UI.

## URL AEM

![URL van console-AEM voor inhoudsfragmenten](./assets/verify/content-fragment-console-url.png){align="center"}

Als u een URL wilt maken die de extensie non-production in AEM koppelt, moet de URL van de AEM UI waarin de extensie wordt geïnjecteerd, worden verkregen. Navigeer naar de AEM as a Cloud Service omgeving om de extensie te controleren en open de interface waarin de extensie moet worden voorvertoond.

Als u bijvoorbeeld een extensie wilt voorvertonen voor de Content Fragment-console:

1. Meld u aan bij de gewenste AEM as a Cloud Service omwenteling.
2. Selecteer de __Inhoudsfragmenten__ pictogram.
3. Wacht tot de AEM Content Fragment Console in de browser is geladen.
4. Kopieer de URL van de AEM Content Fragment Console van de adresbalk van de browser en het lijkt erop:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Deze URL wordt hieronder gebruikt bij het maken van de URL&#39;s voor ontwikkeling en werkgebiedverificatie. Als u de extensie verifieert aan de hand van andere AEM UI&#39;s, vraagt u deze URL&#39;s op en past u onderstaande dezelfde stappen toe.

## Builds voor lokale ontwikkeling controleren

1. Open een opdrachtregel naar de hoofdmap van het extensieproject.
1. De extensie AEM UI uitvoeren als een lokale App Builder-app

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

1. Voeg de volgende twee vraagparams aan toe [URL van AEM gebruikersinterface](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, gewoonlijk `&ext=https://localhost:9080`.

   Twee bovenstaande queryparameters toevoegen (`devMode` en `ext`) als __first__ query-parameters in de URL. AEM verlengbare UI gebruikshash routes (`#/@wknd/aem/...`), zodat de parameters na de `#` werkt niet.

   De voorbeeld-URL moet er als volgt uitzien:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

2. Kopieer en plak de URL van de voorvertoning in uw browser.

   + U moet mogelijk eerst en dan periodiek [het HTTPS-certificaat accepteren](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) voor de host van de lokale toepassing (`https://localhost:9080`).

3. De AEM-interface wordt geladen met de lokale versie van de extensie die in de interface is geïnjecteerd voor verificatie.

>[!IMPORTANT]
>
>Houd er rekening mee dat bij het gebruik van deze benadering de extensie die momenteel wordt ontwikkeld alleen invloed heeft op uw ervaring en dat alle andere gebruikers van de AEM interface de interface zonder de geïnjecteerde extensie ervaren.

## Werkgebiedbuilds verifiëren

1. Open een opdrachtregel naar de hoofdmap van het extensieproject.
1. Controleer of de werkruimte van het werkgebied actief is (of welke werkruimte voor verificatie wordt gebruikt).

   ```shell
   $ aio app use -w Stage
   ```

   Alle wijzigingen samenvoegen in `.env` en `.aio`.

1. Implementeer de bijgewerkte app App Builder. Als u zich niet hebt aangemeld, uitvoeren `aio login` eerst.

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

1. Voeg de volgende twee vraagparams aan toe [URL van AEM gebruikersinterface](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Twee bovenstaande queryparameters toevoegen (`devMode` en `ext`) als __first__ de vraagparameters in URL, als verlengbare AEM UIs gebruiken een knoeiboelroute (`#/@wknd/aem/...`), zodat de parameters na de `#` werkt niet.

   De voorbeeld-URL moet er als volgt uitzien:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Kopieer en plak de URL van de voorvertoning in uw browser.
1. Met de AEM Content Fragment Console wordt de versie van de extensie geïnjecteerd die is geïmplementeerd in de werkruimte van het werkgebied. Deze werkgebied-URL kan ter verificatie worden gedeeld met een kwaliteitscontrole of zakelijke gebruikers.

Houd er rekening mee dat wanneer u deze methode gebruikt, de extensie Staged alleen wordt geïnjecteerd op AEM Content Fragment Console&#39;s wanneer u toegang hebt tot de URL van het werkgebied van het vaartuig.

1. Ingevoerde extensies kunnen worden bijgewerkt door `aio app deploy` en deze wijzigingen worden automatisch doorgevoerd wanneer u de voorbeeld-URL gebruikt.
1. Als u een extensie voor verificatie wilt verwijderen, voert u `aio app undeploy`.

## Voorvertoning bladwijzer

Om het maken van URL&#39;s met voorvertoningen en voorvertoningen zoals hierboven beschreven, te vergemakkelijken, kunt u een JavaScript-bladwijzer maken die de extensie laadt.

De onderstaande bladwijzer bevat een voorvertoning van de [plaatselijke ontwikkelingsgebouwen](#verify-local-development-builds) van de verlenging op `https://localhost:9080`. Aan voorvertoning [werkgebiedbuilds](#verify-stage-builds), maakt u een bladwijzer met de `previewApp` variabele die is ingesteld op de URL van de geïmplementeerde App Builder-app.

1. Maak een bladwijzer in uw browser.
2. Bewerk de bladwijzer.
3. Een bladwijzer een betekenisvolle naam geven, zoals `AEM UI Extension Preview (localhost:9080)`.
4. Stel de URL van de bladwijzer in op de volgende code:

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

5. Navigeer naar een uitbreidbare AEM UI om de voorvertoningsextensie te laden en klik vervolgens op de bladwijzer.

>[!TIP]
>
> Als de extensie App Builder niet wordt geladen bij gebruik, `&ext=https://localhost:9080`, opent u die host en poort rechtstreeks in een browsertabblad en accepteert u het zelfondertekende certificaat. Probeer de bladwijzer opnieuw.
