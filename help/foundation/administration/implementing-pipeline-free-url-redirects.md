---
title: Het uitvoeren van pijpleidingsvrije URL richt
description: Leer hoe u URL-omleidingen zonder pijplijn in AEM as a Cloud Service implementeert, zodat het marketingteam omleidingen kan beheren zonder een ontwikkelaar te hebben.
version: Experience Manager as a Cloud Service
feature: Operations, Dispatcher
topic: Development, Content Management, Administration
role: Developer, User
level: Beginner, Intermediate
doc-type: Article
duration: 0
last-substantial-update: 2025-02-05T00:00:00Z
jira: KT-15739
thumbnail: KT-15739.jpeg
exl-id: 3b0f5971-38b8-4b9e-b90e-9de7432e0e9d
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '973'
ht-degree: 0%

---

# Het uitvoeren van pijpleidingsvrije URL richt

Leer hoe te om [&#x200B; pijpleiding-vrije URL uit te voeren richt &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) in AEM as a Cloud Service om het marketing team toe te laten om de omleidingen te beheren zonder een ontwikkelaar te hoeven.

Er zijn veelvoudige opties om URL te beheren richt in AEM, voor meer informatie, zie [&#x200B; opnieuw richt URL &#x200B;](url-redirection.md).

Het leerprogramma concentreert zich bij het creëren van URL richt zich als zeer belangrijk-waardeparen in een tekstdossier zoals [&#x200B; Apache RewriteMap &#x200B;](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) opnieuw en gebruikt AEM as a Cloud Service specifieke configuratie om hen in de module Apache/Dispatcher te laden.

## Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

- Het milieu van AEM as a Cloud Service met versie **18311 of hoger**.

- Het steekproef [&#x200B; WKND &#x200B;](https://github.com/adobe/aem-guides-wknd) project van Plaatsen moet op het worden opgesteld.

## Gebruiksscenario voor zelfstudie

Voor het demodoel, laten we veronderstellen dat het WKND marketing team een nieuwe skicampagne lanceert. Ze willen korte URL&#39;s maken voor de ski-avontuurpagina&#39;s en deze op hun eigen manier beheren, net zoals ze de inhoud beheren. Zij besloten om de [&#x200B; pijpleiding-vrije URL te gebruiken herleidt &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) benadering om URL te beheren opnieuw richt.

Gebaseerd op de vereisten van het marketing team, zijn het volgende URL omleidingen die moeten worden gecreeerd.

| SOURCE URL | Doel-URL |
|------------|------------|
| /ski | /us/en/adventures.html |
| /ski/northamerica | /us/en/adventures/downhill-skiing-wyoming.html |
| /ski/westkust | /us/en/adventures/tahoe-skiing.html |
| /ski/europe | /us/en/adventures/ski-touring-mont-blanc.html |

Nu, zie hoe te om deze URL te beheren richt en vereiste eenmalig Dispatcher configuraties in de milieu van AEM as a Cloud Service.

## URL-omleidingen beheren{#manage-redirects}

Om URL te beheren zijn er veelvoudige beschikbare opties, onderzoeken hen.

### Tekstbestand in DAM

De URL-omleidingen kunnen worden beheerd als sleutelwaardeparen in een tekstbestand en worden geüpload naar het DAM (AEM Digital Asset Management).

De bovenstaande URL-omleidingen kunnen bijvoorbeeld worden opgeslagen in een tekstbestand met de naam `skicampaign.txt` en worden geüpload naar de map DAM @ `/content/dam/wknd/redirects` . Na revisie en goedkeuring kan het marketingteam het tekstbestand publiceren.

```
# Ski Campaign Redirects separated by the TAB character
/ski      /us/en/adventures.html
/ski/northamerica  /us/en/adventures/downhill-skiing-wyoming.html
/ski/westcoast   /us/en/adventures/tahoe-skiing.html
/ski/europe          /us/en/adventures/ski-touring-mont-blanc.html
```

![&#x200B; dossier van de Tekst in DAM &#x200B;](./assets/pipeline-free-redirects/text-file-in-dam.png)

### ACS-opdrachten - Kaartbeheer omleiden

[&#x200B; ACS Commons - richt de Manager van de Kaart &#x200B;](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) opnieuw om een gebruikersvriendelijke interface om URL te leiden richt.

Bijvoorbeeld, kan het marketing team een nieuwe *Redirect genoemde pagina van Kaarten* `SkiCampaign` tot stand brengen en bovengenoemde URL toe leidt opnieuw richt gebruikend **geeft Ingangen** tabel uit. De URL-omleidingen zijn beschikbaar via `/etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt` .

![&#x200B; Redirect de Manager van de Kaart &#x200B;](./assets/pipeline-free-redirects/redirect-map-manager.png)

>[!IMPORTANT]
>
>ACS de versie van de Bevelen **6.7.0 of hoger** wordt vereist om de Redirect Manager van de Kaart te gebruiken, voor meer informatie, zie de [&#x200B; Bevelen ACS - Redirect Manager &#x200B;](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html).

### ACS-opdrachten - Omleidingsbeheer

Alternatief, verstrekken de [&#x200B; ACS Bevelen - Richt Manager &#x200B;](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) ook een gebruikersvriendelijke interface om URL te beheren richt.

Bijvoorbeeld, kan het marketing team een nieuwe configuratie tot stand brengen genoemd `/conf/wknd` en de bovengenoemde omleidingen toevoegen URL gebruikend de **+ knoop van de Configuratie van de Omleiding**. De URL-omleidingen zijn beschikbaar via `/conf/wknd/settings/redirects.txt` .

![&#x200B; Redirect Manager &#x200B;](./assets/pipeline-free-redirects/redirect-manager.png)

>[!IMPORTANT]
>
>ACS de versie van de Bevelen **6.10.0 of hoger** wordt vereist om de Redirect Manager te gebruiken, voor meer informatie, zie de [&#x200B; Bevelen ACS - Redirect Manager &#x200B;](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html).

## De Dispatcher configureren

Als u de URL als een RewriteMap wilt laden en deze wilt toepassen op binnenkomende aanvragen, zijn de volgende Dispatcher-configuraties vereist.

### Dispatcher-module inschakelen voor flexibele modus

Eerst, verifieer dat de module van Dispatcher voor _flexibele wijze_ wordt toegelaten. De aanwezigheid van `USE_SOURCES_DIRECTLY` -bestand in de `dispatcher/src/opt-in` -map geeft aan dat de Dispatcher zich in de flexibele modus bevindt.

### URL-omleidingen laden als RewriteMap

Maak vervolgens een nieuw configuratiebestand `managed-rewrite-maps.yaml` in de map `dispatcher/src/opt-in` met de volgende structuur.

```yaml
maps:
- name: <MAPNAME>.map # e.g. skicampaign.map
    path: <ABSOLUTE_PATH_TO_URL_REDIRECTS_FILE> # e.g. /content/dam/wknd/redirects/skicampaign.txt, /etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt, /conf/wknd/settings/redirects.txt
    wait: false # Optional, default is false, when true, the Apache waits for the map to be loaded before starting
    ttl: 300 # Optional, default is 300 seconds, the reload interval for the map
```

Tijdens de implementatie maakt de Dispatcher het `<MAPNAME>.map` -bestand in de map `/tmp/rewrites` .

>[!IMPORTANT]
>
> De bestandsnaam (`managed-rewrite-maps.yaml`) en de locatie (`dispatcher/src/opt-in`) moeten exact zo zijn als hierboven is vermeld. Beschouw het als een te volgen conventie.

### URL-omleidingen toepassen op binnenkomende aanvragen

Tot slot creeer of werk het Apache herschrijf configuratiedossier bij om de bovengenoemde kaart (`<MAPNAME>.map`) te gebruiken. Laten we bijvoorbeeld het bestand `rewrite.rules` uit de map `dispatcher/src/conf.d/rewrites` gebruiken om de URL-omleidingen toe te passen.

```
...
# Use the RewriteMap to define the URL redirects
RewriteMap <MAPALIAS> dbm=sdbm:/tmp/rewrites/<MAPNAME>.map

RewriteCond ${<MAPALIAS>:$1} !=""
RewriteRule ^(.*)$ ${<MAPALIAS>:$1|/} [L,R=301]    
...
```

### Voorbeelden van configuraties

Laten wij de configuraties van Dispatcher voor elk van de hierboven vermelde opties van het omleidingsbeheer URL [&#x200B; &#x200B;](#manage-redirects) herzien.

>[!BEGINTABS]

>[!TAB  dossier van de Tekst in DAM ]

Wanneer de URL-omleidingen worden beheerd als sleutelwaardeparen in een tekstbestand en worden geüpload naar de DAM, zijn de configuraties als volgt.

[!BADGE &#x200B; dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="Bestandsnaam van codevoorbeeld hieronder."}

```yaml
maps:
- name: skicampaign.map
  path: /content/dam/wknd/redirects/skicampaign.txt
```

[!BADGE &#x200B; dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="Bestandsnaam van codevoorbeeld hieronder."}

```
...

# The DAM-managed skicampaign.txt file as skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:$1} !=""
RewriteRule ^(.*)$ ${skicampaign:$1|/} [L,R=301]

...
```

>[!TAB  ACS Commons - richt de Manager van de Kaart ]

Wanneer de URL-omleidingen worden beheerd met de ACS-opdrachten - Omleiding Kaartbeheer, zijn de configuraties als volgt.

[!BADGE &#x200B; dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="Bestandsnaam van codevoorbeeld hieronder."}

```yaml
maps:
- name: skicampaign.map
  path: /etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt
```

[!BADGE &#x200B; dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="Bestandsnaam van codevoorbeeld hieronder."}

```
...

# The Redirect Map Manager-managed skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:$1} !=""
RewriteRule ^(.*)$ ${skicampaign:$1|/} [L,R=301]

...
```

>[!TAB ACS Commons - richt Manager  opnieuw]

Wanneer de omleiding URL gebruikend ACS Commons - de Manager van de Omleiding wordt beheerd, zijn de configuraties als volgt.

[!BADGE &#x200B; dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="Bestandsnaam van codevoorbeeld hieronder."}

```yaml
maps:
- name: skicampaign.map
  path: /conf/wknd/settings/redirects.txt
```

[!BADGE &#x200B; dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="Bestandsnaam van codevoorbeeld hieronder."}

```
...

# The Redirect Manager-managed skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:$1} !=""
RewriteRule ^(.*)$ ${skicampaign:$1|/} [L,R=301]

...
```

>[!ENDTABS]

## Hoe te om de configuraties op te stellen

>[!IMPORTANT]
>
>De *pijpleiding-vrije* termijn wordt gebruikt om te benadrukken dat de configuraties *slechts eenmaal* worden opgesteld en het marketing team kan URL omleiden door het tekstdossier bij te werken.

Om de configuraties op te stellen, gebruik de [&#x200B; volledig-stapel &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline) of [&#x200B; 3&rbrace; pijpleiding van de Webrij config &lbrace;in &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#web-tier-config-pipelines) Cloud Manager [.](https://my.cloudmanager.adobe.com/)

![&#x200B; Opname via volledig-stapelpijpleiding &#x200B;](./assets/pipeline-free-redirects/deploy-full-stack-pipeline.png)


Zodra de plaatsing succesvol is, zijn de omleidingen URL actief en het marketing team kan hen beheren zonder een ontwikkelaar te hebben nodig.

## De omleidingen van de URL testen

Test de URL-omleidingen met de browser of de opdracht `curl` . Open de URL van `/ski/westcoast` en controleer of deze wordt doorgestuurd naar `/us/en/adventures/tahoe-skiing.html` .

## Samenvatting

In deze zelfstudie hebt u geleerd hoe u URL-omleidingen kunt beheren met behulp van configuraties zonder pijplijn in AEM as a Cloud Service-omgeving.

Het marketingteam kan de URL omleiden als sleutel-waardeparen in een tekstbestand en deze uploaden naar DAM of de ACS-commons - Redirect Map Manager of Redirect Manager gebruiken. De configuraties van Dispatcher worden bijgewerkt om URL te laden herleidt als RewriteMap en hen op de inkomende verzoeken toe te passen.

## Aanvullende bronnen

- [&#x200B; lijn-vrije URL richt zich opnieuw &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects)
- [URL-omleidingen](url-redirection.md)
