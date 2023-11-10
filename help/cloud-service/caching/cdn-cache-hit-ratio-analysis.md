---
title: Analyse van de CDN-cache-raakverhouding
description: Leer hoe u de AEM as a Cloud Service CDN-logboeken analyseert. Verbeter inzichten zoals geheim voorgeheugenklapverhouding, en hoogste URLs van MISS en PASS geheim voorgeheugentypes voor optimalisatiedoeleinden.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-11-10T00:00:00Z
jira: KT-13312
thumbnail: KT-13312.jpeg
source-git-commit: bfc4d843c53373010ee04cfa590272cedea7a686
workflow-type: tm+mt
source-wordcount: '1232'
ht-degree: 0%

---


# Analyse van de CDN-cache-raakverhouding

Leer hoe u de as a Cloud Service AEM analyseert **CDN-logbestanden** en meer inzicht te krijgen, zoals **cacheverhouding**, en **top-URL&#39;s van _MISS_ en _PASS_ cachetypen** voor optimalisatie.


De CDN-logboeken zijn beschikbaar in de JSON-indeling, die verschillende velden bevat, waaronder `url`, `cache`voor meer informatie raadpleegt u de [CDN-logboekindeling](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/logging.html?lang=en#cdn-log:~:text=Toggle%20Text%20Wrapping-,Log%20Format,-The%20CDN%20logs). De `cache` veld bevat informatie over _status van de cache_ en de mogelijke waarden ervan zijn HIT, MISS of PASS. Laten we de details van mogelijke waarden bekijken.

| Status van cache </br> Mogelijke waarde | Beschrijving |
|------------------------------------|:-----------------------------------------------------:|
| HIT | De gevraagde gegevens zijn _gevonden in de CDN-cache en hoeft niet te worden opgehaald_ verzoek aan de AEM server. |
| MISS | De gevraagde gegevens zijn _niet gevonden in de CDN-cache en moet worden aangevraagd_ van de AEM server. |
| PASS | De gevraagde gegevens zijn _expliciet ingesteld op niet in cache plaatsen_ en altijd van de AEM server worden opgehaald. |

Voor deze zelfstudie [AEM WKND-project](https://github.com/adobe/aem-guides-wknd) wordt ingezet in de AEM as a Cloud Service omgeving en er wordt een kleine prestatietest geactiveerd met behulp van [Apache JMeter](https://jmeter.apache.org/).

## CDN-logbestanden downloaden

Ga als volgt te werk om de CDN-logboeken te downloaden:

1. Aanmelden bij Cloud Manager [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) en selecteert u uw organisatie en programma.

1. Voor een gewenste AEMCS-omgeving selecteert u **Logbestanden downloaden** in het ovaalmenu.

   ![Logbestanden downloaden - Cloud Manager](assets/cdn-logs-analysis/download-logs.png){width="200" zoomable="yes"}

1. In de **Logbestanden downloaden** selecteert u de **Publiceren** De dienst van het drop-down menu, dan klik het downloadpictogram naast **cdn** rij.

   ![CDN-logbestanden - Cloud Manager](assets/cdn-logs-analysis/download-cdn-logs.png){width="200" zoomable="yes"}


Als het gedownloade logbestand afkomstig is van _vandaag_ de bestandsextensie is `.log` anders voor oudere logbestanden is de extensie `.log.gz`.

## Gedownloade CDN-logboeken analyseren

Om inzichten zoals geheim voorgeheugenklapverhouding, en hoogste URLs van MISS en PASS geheim voorgeheugentypes te bereiken analyseert het gedownloade CDN logboekdossier. Deze inzichten helpen om [CDN-cacheconfiguratie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html) en verbetert u de prestaties van de site.

Voor het analyseren van de CDN-logboeken gebruikt dit artikel de **Elasticsearch, Logstash en Kibana (ELK)** [dashboardgereedschap](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) en [Jupyter-laptop](https://jupyter.org/).


### De dashboardgereedschappen gebruiken

De [ELK-stapel](https://www.elastic.co/elastic-stack) is een reeks hulpmiddelen die een scalable oplossing verstrekken om, de gegevens te zoeken te analyseren en te visualiseren. Het bestaat uit Elasticsearch, Logstash, en Kibana.

Om de belangrijkste details te identificeren, gebruiken wij [AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) dashboard-toolingproject. Dit project verstrekt een container van de Dok van de stapel van ELK en een vooraf gevormd dashboard van Kibana om de CDN- logboeken te analyseren.

1. Voer de volgende stappen uit [De ELK Docker-container instellen](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool#how-to-set-up-the-elk-docker-container) en zorg ervoor dat u de **Hoogte-breedteverhouding CDN-cache** Kibana dashboard.

1. Ga als volgt te werk om de CDN-raakverhouding in cache en URL&#39;s als bovenste URL te identificeren:

   1. Kopieer het gedownloade CDN-logbestand of de gedownloade CDN-logbestanden in de omgevingsspecifieke map.

   1. Open de **Hoogte-breedteverhouding CDN-cache** Klik op Hamburger Menu > Analytics > Dashboard > CDN Cache Hit Ratio.

      ![CDN-cachehoogte-breedteverhouding - Kibana-dashboard](assets/cdn-logs-analysis/cdn-cache-hit-ratio-dashboard.png){width="200" zoomable="yes"}

   1. Selecteer het gewenste tijdbereik in de rechterbovenhoek.

      ![Tijdbereik - Kibana-dashboard](assets/cdn-logs-analysis/time-range.png){width="200" zoomable="yes"}

   1. De **Hoogte-breedteverhouding CDN-cache** Het dashboard spreekt voor zich.

   1. De _Totale verzoekanalyse_ in deze sectie worden de volgende details weergegeven:
      - Cacheverhoudingen per cachetype
      - Cacheaantallen per cachetype

      ![Totale aanvraaganalyse - Kibana-dashboard](assets/cdn-logs-analysis/total-request-analysis.png){width="200" zoomable="yes"}

   1. De _Analyse op verzoek of MIME-typen_ geeft de volgende details weer:
      - Cacheverhoudingen per cachetype
      - Cacheaantallen per cachetype
      - MISS- en PASS-URL&#39;s bovenaan

      ![Analyse op verzoek- of MIME-typen - Kibana-dashboard](assets/cdn-logs-analysis/analysis-by-request-or-mime-types.png){width="200" zoomable="yes"}

#### Filteren op omgevingsnaam of programma-id

Voer de volgende stappen uit om de opgenomen logs te filteren op de naam van de omgeving:

1. Klik in het dashboard voor de hoogte-breedteverhouding van CDN-cache op de knop **Filter toevoegen** pictogram.

   ![Filter - Kibana-dashboard](assets/cdn-logs-analysis/filter.png){width="200" zoomable="yes"}

1. In de **Filter toevoegen** modal, selecteer `aem_env_name.keyword` in het keuzemenu, en `is` operator en gewenste omgevingsnaam voor volgende veld en klik ten slotte op _Filter toevoegen_.

   ![Filter toevoegen - Kibana-dashboard](assets/cdn-logs-analysis/add-filter.png){width="200" zoomable="yes"}

#### Filteren op hostnaam

Voer de volgende stappen uit om de opgenomen logbestanden te filteren op hostnaam:

1. Klik in het dashboard voor de hoogte-breedteverhouding van CDN-cache op de knop **Filter toevoegen** pictogram.

   ![Filter - Kibana-dashboard](assets/cdn-logs-analysis/filter.png){width="200" zoomable="yes"}

1. In de **Filter toevoegen** modal, selecteer `host.keyword` in het keuzemenu, en `is` operator en gewenste hostnaam voor volgende veld en klik ten slotte op _Filter toevoegen_.

   ![Hostfilter - Kibana-dashboard](assets/cdn-logs-analysis/add-host-filter.png){width="200" zoomable="yes"}

Voeg op basis van de analysevereisten ook meer filters toe aan het dashboard.

### Jupyter-laptop gebruiken

De [Jupyter-laptop](https://jupyter.org/) is een opensource webtoepassing waarmee u documenten kunt maken die code, tekst en visualisatie bevatten. Het wordt gebruikt voor gegevenstransformatie, visualisatie, en statistische modellering.

Als u de CDN-logbestandsanalyse wilt versnellen, downloadt u de [AEM-as-a-CloudService - CDN Logs Analysis - Jupyter-laptop](./assets/cdn-logs-analysis/aemcs_cdn_logs_analysis.ipynb) bestand.

De gedownloade `aemcs_cdn_logs_analysis.ipynb` Het dossier van de &quot;Interactive Python Laptop&quot;spreekt voor zich, echter de belangrijkste hoogtepunten van elke sectie zijn:

- **Extra bibliotheken installeren**: installeert de `termcolor` en `tabulate` Pythonbibliotheken.
- **CDN-logbestand laden**: laadt het CDN-logbestand met `log_file` variabele waarde, zorg ervoor om zijn waarde bij te werken. Het zet ook dit CDN- logboek in [Pandas DataFrame](https://pandas.pydata.org/docs/reference/frame.html).
- **Analyse uitvoeren**: het eerste codeblok is _Resultaat van de Analyse van de vertoning voor Totaal, HTML, JS/CSS en de Verzoeken van het Beeld_, biedt het een percentage voor de aanraakverhouding in het cache, een balk en schijfgrafieken.
Het tweede codeblok is _Hoogste 5 MISS en PASS verzoek URLs voor HTML, JS/CSS, en Beeld_, worden URL&#39;s en hun aantallen weergegeven in tabelindeling.

#### Jupyter-laptop uitvoeren in Experience Platform

>[!IMPORTANT]
>
>Als u het Experience Platform gebruikt of een licentie geeft, kunt u de Jupyter-laptop uitvoeren zonder extra software te installeren.

Voer de volgende stappen uit om het Jupyter-notebook in Experience Platform uit te voeren:

1. Aanmelden bij de [Adobe Experience Cloud](https://experience.adobe.com/), op de startpagina > **Snelle toegang** sectie > klik op **Experience Platform**

   ![Experience Platform](assets/cdn-logs-analysis/experience-platform.png){width="200" zoomable="yes"}

1. Klik op de startpagina van Adobe Experience Platform > sectie Data Science > op de knop **Laptops** menu-item. Als u de Jupyter-laptopomgeving wilt starten, klikt u op de knop **JupyterLab** tab.

   ![Update voor de waarde van het logboekbestand voor laptops](assets/cdn-logs-analysis/datascience-notebook.png){width="200" zoomable="yes"}

1. Gebruik in het menu JupyterLab de opdracht **Bestanden uploaden** het gedownloade CDN-logbestand uploaden en `aemcs_cdn_logs_analysis.ipynb` bestand.

   ![Bestanden uploaden - JupyteLab](assets/cdn-logs-analysis/jupyterlab-upload-file.png){width="200" zoomable="yes"}

1. Open de `aemcs_cdn_logs_analysis.ipynb` door te dubbelklikken.

1. In de **CDN-logbestand laden** van de laptop, werkt u de `log_file` waarde.

   ![Update voor de waarde van het logboekbestand voor laptops](assets/cdn-logs-analysis/notebook-update-variable.png){width="200" zoomable="yes"}

1. Als u de geselecteerde cel wilt uitvoeren en verder wilt gaan, klikt u op de knop **Afspelen** pictogram.

   ![Update voor de waarde van het logboekbestand voor laptops](assets/cdn-logs-analysis/notebook-run-cell.png){width="200" zoomable="yes"}

1. Nadat u het **Resultaat van de Analyse van de vertoning voor Totaal, HTML, JS/CSS, en de Verzoeken van het Beeld** codecel, toont de output het percentage van de geheim voorgeheugentreffelijkheid, bar, en cirkeldiagrammen.

   ![Update voor de waarde van het logboekbestand voor laptops](assets/cdn-logs-analysis/output-cache-hit-ratio.png){width="200" zoomable="yes"}

1. Nadat u het **Hoogste 5 MISS en PASS verzoek URLs voor HTML, JS/CSS, en Beeld** De codecel, de output toont Hoogste 5 MISS en PASS Verzoek URLs.

   ![Update voor de waarde van het logboekbestand voor laptops](assets/cdn-logs-analysis/output-top-urls.png){width="200" zoomable="yes"}

U kunt het Notitieboekje van de Jupyter verbeteren om de logboeken te analyseren CDN die op uw vereisten worden gebaseerd.

## CDN-cacheconfiguratie optimaliseren

Nadat u de CDN-logboeken hebt geanalyseerd, kunt u de CDN-cacheconfiguratie optimaliseren om de siteprestaties te verbeteren. De AEM beste praktijken moeten een geheim voorgeheugenklapverhouding van 90% of hoger hebben.

Zie voor meer informatie [CDN-cacheconfiguratie optimaliseren](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#caching).

Het AEM WKND-project heeft een referentie-CDN-configuratie, voor meer informatie, zie [CDN-configuratie](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L137-L190) van de `wknd.vhost` bestand.
