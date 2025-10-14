---
title: Een voorvertoning weergeven van een extensie van Universal Editor
description: Leer hoe u tijdens de ontwikkeling een voorvertoning kunt weergeven van een extensie van Universal Editor die lokaal wordt uitgevoerd.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner, Intermediate, Experienced
doc-type: Tutorial
jira: KT-18658
source-git-commit: f0ad5d66549970337118220156d7a6b0fd30fd57
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---


# Een voorvertoning weergeven van een lokale extensie van Universal Editor

>[!TIP]
> Leer hoe te [&#x200B; tot een Universele uitbreiding van de Redacteur &#x200B;](https://developer.adobe.com/uix/docs/services/aem-universal-editor/) leiden.

Als u een voorvertoning van de extensie Universal Editor wilt weergeven tijdens de ontwikkeling, moet u:

1. Voer de extensie lokaal uit.
2. Accepteer het zelfondertekende certificaat.
3. Open een pagina in Universele Redacteur.
4. Werk de locatie-URL bij om de lokale extensie te laden.

## De extensie lokaal uitvoeren

Dit veronderstelt u reeds de uitbreiding van de Redacteur van de a [&#x200B; Universele &#x200B;](https://developer.adobe.com/uix/docs/services/aem-universal-editor/) hebt gecreeerd en het willen voorproef terwijl het testen en het ontwikkelen plaatselijk.

Start de extensie Universal Editor met:

```bash
$ aio app run
```

De uitvoer ziet er als volgt uit:

```
To view your local application:
  -> https://localhost:9080
To view your deployed application in the Experience Cloud shell:
  -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
```

Hierdoor wordt de extensie standaard uitgevoerd op `https://localhost:9080` .


## Het zelfondertekende certificaat accepteren

Voor de Universal Editor is HTTPS vereist om extensies te laden. Aangezien bij lokale ontwikkeling een zelfondertekend certificaat wordt gebruikt, moet uw browser dit expliciet vertrouwen.

Open een nieuw browsertabblad en navigeer met de opdracht `aio app run` naar de URL-uitvoer van de lokale extensie:

```
https://localhost:9080
```

Er verschijnt een certificaatwaarschuwing in uw browser. Accepteer het certificaat om verder te gaan.

![&#x200B; Accepteer het zelf-ondertekende certificaat &#x200B;](./assets/local-extension-preview/accept-certificate.png)

Nadat de lokale extensie is geaccepteerd, ziet u de tijdelijke aanduiding voor de pagina:

![&#x200B; de Uitbreiding is toegankelijk &#x200B;](./assets/local-extension-preview/extension-accessible.png)


## Een pagina openen in de Universal Editor

Open Universele Redacteur via de [&#x200B; Universele console van de Redacteur &#x200B;](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/) of door een pagina in AEM Sites uit te geven die Universele Redacteur gebruikt:

![&#x200B; open een pagina in Universele Redacteur &#x200B;](./assets/local-extension-preview/open-page-in-ue.png)


## De extensie laden

In Universele Redacteur, bepaal de plaats van het **gebied van de Plaats** bij het hoogste centrum van de interface. Breid het uit en werk **URL op het gebied van de Plaats** bij, **niet de browser adresbar**.

Voeg de volgende queryparameters toe:

* `devMode=true` - Schakelt de ontwikkelingsmodus in voor de universele editor.
* `ext=https://localhost:9080` - Laadt uw lokaal lopende uitbreiding.

Voorbeeld:

```
https://author-pXXX-eXXX.adobeaemcloud.com/content/aem-ue-wknd/index.html?devMode=true&ext=https://localhost:9080
```

![&#x200B; werk de Universele plaats URL van de Redacteur &#x200B;](./assets/local-extension-preview/update-location-url.png) bij


## Voorvertoning van de extensie weergeven

Voer a **hard opnieuw laden** van browser uit om bijgewerkte URL te verzekeren wordt gebruikt.

De Universal Editor laadt nu uw lokale extensie, alleen in uw browsersessie.

Wijzigingen die u lokaal aanbrengt, worden direct doorgevoerd.

![&#x200B; Lokale die uitbreiding &#x200B;](./assets/local-extension-preview/extension-loaded.png) wordt geladen

