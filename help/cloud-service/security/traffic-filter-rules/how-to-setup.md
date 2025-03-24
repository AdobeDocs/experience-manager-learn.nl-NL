---
title: Hoe te de regels van de Filter van het Opstelling van het Verkeer met inbegrip van de regels van WAF
description: Leer hoe te opstelling om, de resultaten van de regels van de Filter van het Verkeer tot stand te brengen, op te stellen te testen en te analyseren met inbegrip van de regels van WAF.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: b67bf642-3341-48d0-8ea9-5f262febf414
duration: 292
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 0%

---

# Hoe te de regels van de Filter van het Opstelling van het Verkeer met inbegrip van de regels van WAF

Leer **hoe te opstelling** de regels van de verkeersfilter, met inbegrip van de regels van WAF. Lees over het maken, implementeren, testen en analyseren van resultaten.

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## Instellen

Het installatieproces omvat het volgende:

- _creërend regels_ met een aangewezen het projectstructuur en configuratiedossier van AEM.
- _het opstellen van regels_ gebruikend de configuratiepijplijn van Adobe Cloud Manager.
- _testende regels_ gebruikend diverse hulpmiddelen om verkeer te produceren.
- _analyserend de resultaten_ gebruikend de logboeken van AEMCS CDN en dashboardtooling.

### Regels maken in uw AEM-project

Ga als volgt te werk om regels te maken:

1. Maak een map `config` op het hoofdniveau van uw AEM-project.

1. Maak in de map `config` een nieuw bestand met de naam `cdn.yaml` .

1. Voeg de volgende metagegevens toe aan het `cdn.yaml` -bestand:

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
```

Zie een voorbeeld van het `cdn.yaml` -bestand in het AEM Guides WKND Sites Project:

![ WKND het dossier en de omslag van de het projectregels van AEM ](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### Regels implementeren via Cloud Manager {#deploy-rules-through-cloud-manager}

Voer de volgende stappen uit om de regels te implementeren:

1. Logboek in Cloud Manager bij [ my.cloudmanager.adobe.com ](https://my.cloudmanager.adobe.com/) en selecteert de aangewezen organisatie en het programma.

1. Navigeer aan de _Pipelines_ kaart van de _pagina van het Overzicht van het Programma_ en klik **+ voeg** knoop toe en selecteer het gewenste pijpleidingstype.

   ![ de Pipelinekaart van Cloud Manager ](./assets/cloud-manager-pipelines-card.png)

   In het voorbeeld hierboven, voor demodoeleinden _voeg niet-Productiepijpleiding_ toe wordt geselecteerd aangezien een ontwikkelomgeving wordt gebruikt.

1. In _voeg de dialoog van de Pijpleiding van de Niet-Productie_ toe, kies en ga de volgende details in:

   1. Configuratiestap:

      - **Type**: De Pijpleiding van de Plaatsing
      - **Naam van de Pijpleiding**: Dev-Config

      ![ Cloud Manager Config de dialoog van de Pijpleiding ](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. Stap Source-code:

      - **Code om** op te stellen: Gerichte plaatsing
      - **omvatten**: Config
      - **Milieu van de Plaatsing**: Naam van uw milieu, bijvoorbeeld, wknd-programma-dev.
      - **Bewaarplaats**: De bewaarplaats van het Git van waar de pijpleiding de code zou moeten terugwinnen; bijvoorbeeld, `wknd-site`
      - **de Tak van de Git**: De naam van de de bewaarplaats van de Git tak.
      - **Plaats van de Code**: `/config`, die aan de top-level configuratiemap beantwoordt die in de vorige stap wordt gecreeerd.

      ![ Cloud Manager Config de dialoog van de Pijpleiding ](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### Regels testen door verkeer te genereren

Om regels te testen, zijn er verschillende hulpmiddelen van derden beschikbaar en uw organisatie kan een aangewezen hulpmiddel hebben. Voor het demodoel, gebruiken de volgende hulpmiddelen:

- [ Kromme ](https://curl.se/) voor basis het testen als het aanhalen van een URL en het controleren van de antwoordcode.

- [ Vegeta ](https://github.com/tsenart/vegeta) om ontkenning van de dienst (DOS) uit te voeren. Volg de installatieinstructies van [ Vegeta GitHub ](https://github.com/tsenart/vegeta#install).

- [ Nikto ](https://github.com/sullo/nikto/wiki) om potentiële problemen en veiligheidskwetsbaarheid zoals XSS, SQL injectie, en meer te vinden. Volg installatieinstructies van [ Nikto GitHub ](https://github.com/sullo/nikto).

- Controleer of de gereedschappen zijn geïnstalleerd en beschikbaar zijn in uw terminal door de onderstaande opdrachten uit te voeren:

  ```shell
  # Curl version check
  $ curl --version
  
  # Vegeta version check
  $ vegeta -version
  
  # Nikto version check
  $ cd <PATH-OF-CLONED-REPO>/program
  ./nikto.pl -Version
  ```

### Resultaten analyseren met de dashboardgereedschappen

Na het creëren van, het opstellen van, en het testen van de regels, kunt u de resultaten analyseren gebruikend **CDN** logboeken en **AEMCS-CDN-Logboek-Analyse-Tooling**. Het gereedschap biedt een set dashboards om de resultaten voor de stapel Splunk en ELK (Elasticsearch, Logstash en Kibana) te visualiseren.

Gereedschap kan van de [ AEMCS-CDN-Logboek-Analyse-Tooling ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) bewaarplaats worden gekloond GitHub. Dan, volg de instructies om het **dashboard van het Verkeer CDN** en **dashboards van WAF** voor uw aangewezen observatiehulpmiddel te installeren en te laden.

In deze zelfstudie gebruiken we de ELK-stapel. Volg de [ ELK container van het Dok voor de Analyse van het Logboek van AEMCS CDN ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md) instructies aan opstelling de stapel van ELK.

- Nadat u het voorbeelddashboard hebt geladen, ziet de gereedschapspagina Elastic er als volgt uit:

  ![ het Dashboard van de Regels van de Filterregels van het Verkeer van de ELK ](./assets/elk-dashboard.png)

>[!NOTE]
>
>    Aangezien er nog geen AEMCS CDN-logbestanden worden opgenomen, is het dashboard leeg.


## Volgende stap

Leer hoe te om de regels van de verkeersfilter met inbegrip van de regels van WAF in het [ Voorbeelden en hoofdstuk van de resultaatanalyse ](./examples-and-analysis.md) te verklaren, gebruikend het Project van de Plaatsen van AEM WKND.
