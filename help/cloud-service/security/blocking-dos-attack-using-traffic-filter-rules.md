---
title: Het blokkeren Dos en de aanvallen van DDoS gebruikend de regels van de verkeersfilter
description: Leer hoe te om aanvallen te blokkeren van Dos en van DDoS gebruikend de regels van de verkeersfilter bij de AEM as a Cloud Service verstrekte CDN.
version: Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 4111ae0cf8777ce21c224991b8b1c66fb01041b3
workflow-type: tm+mt
source-wordcount: '1968'
ht-degree: 0%

---

# Het blokkeren Dos en de aanvallen van DDoS gebruikend de regels van de verkeersfilter

Leer hoe te om Ontkenning van de Ontkenning van de Dienst (Dos) en Verdeelde Ontkenning van de aanvallen van de Dienst (DDoS) te blokkeren gebruikend **snelheidslimietverkeersfilter** regels en andere strategieën bij de as a Cloud Service AEM (AEMCS) beheerde CDN. Deze aanvallen veroorzaken verkeerspikes bij CDN en potentieel bij de AEM publicatiedienst (alias oorsprong) en kunnen plaatsontvankelijkheid en beschikbaarheid beïnvloeden.

Deze zelfstudie dient als richtlijn voor _hoe te om uw verkeerspatronen te analyseren en tariefgrens te vormen [verkeersfilterregels](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)_ om die aanvallen te verlichten. In de zelfstudie wordt ook beschreven hoe [waarschuwingen configureren](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) zodat u op de hoogte wordt gebracht wanneer er een vermoedelijke aanval is.

## Bescherming

Laten we de standaard DoS-beveiliging voor uw AEM website begrijpen:

- **Caching:** Met goed in het voorgeheugen onderbrengend beleid, is het effect van een aanval DDoS beperkter omdat CDN de meeste verzoeken verhindert naar de oorsprong te gaan en prestatiesdegradatie te veroorzaken.
- **Automatisch schalen:** De AEM auteur en publiceert de diensten autoscale om verkeerspikes te behandelen, hoewel zij nog door plotselinge, massale verhogingen van verkeer kunnen worden beïnvloed.
- **Blokkeren:** De Adobe CDN blokkeert verkeer aan de oorsprong als het een Adobe-bepaald tarief van een bepaald IP adres, per CDN PoP (Punt van Aanwezigheid) overschrijdt.
- **Waarschuwing:** Het Centrum van Acties verzendt een verkeerspiek bij het bericht van de oorsprongsalarm wanneer het verkeer een bepaald tarief overschrijdt. Deze waarschuwing wordt geactiveerd wanneer het verkeer naar een CDN PoP groter is dan een _Adobe gedefinieerd_ aanvraagtarief per IP adres. Zie [Waarschuwing verkeersfilterregels](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) voor meer informatie .

Deze ingebouwde bescherming zou als basislijn voor de capaciteit van een organisatie moeten worden beschouwd om het prestatieseffect van een aanval te minimaliseren DDoS. Aangezien elke website andere prestatiekenmerken heeft en kan zien dat de prestaties achteruitgaan voordat aan de door de Adobe gedefinieerde snelheidslimiet wordt voldaan, wordt aanbevolen de standaardbeveiliging uit te breiden tot _klantconfiguratie_.

Laten we eens kijken naar enkele aanvullende, aanbevolen maatregelen die klanten kunnen nemen om hun websites te beschermen tegen aanvallen van Digital Publishing Suite:

- Decreeren **regels voor snelheidsbeperking voor verkeersfilterregels** om verkeer te blokkeren dat een bepaald tarief van één enkel IP adres, per PoP overschrijdt. Dit is doorgaans een lagere drempelwaarde dan de door de Adobe gedefinieerde tarieflimiet.
- Configureren **waarschuwingen** op de regels van de het verkeersfilter van de tariefgrens door een &quot;waakzame actie&quot;zodat wanneer de regel wordt teweeggebracht, wordt een bericht van het Centrum van Acties verzonden.
- Cachedekking verhogen door declareren **aanvraagtransformaties** om queryparameters te negeren.

>[!NOTE]
>
>De [verkeersfilterregelwaarschuwingen](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) -functie is nog niet vrijgegeven. Voor toegang via het programma voor vroege adoptie kunt u e-mail **<aemcs-waf-adopter@adobe.com>**.

### Snelheidlimiet verkeersregelvariaties {#rate-limit-variations}

Er zijn twee variaties van tarief grensverkeersregels:

1. Rand - blokverzoeken die op het tarief van al verkeer (met inbegrip van dat van CDN geheim voorgeheugen) worden gebaseerd, voor bepaalde IP, per PoP.
1. Oorsprong - blokverzoeken die op het tarief van verkeer worden gebaseerd dat voor de oorsprong, voor een bepaald IP, per PoP wordt bestemd.

## Klantenreis

De onderstaande stappen weerspiegelen het proces dat klanten waarschijnlijk zullen doorlopen om hun websites te beschermen.

1. Erken de behoefte aan een regel van de het verkeersfilter van de snelheidsbeperking. Dit kan het gevolg zijn van het ontvangen van de uit-van-de-doos verkeersopwinding bij oorsprongsalarm van de Adobe, of het kan een pro-actieve beslissing zijn om voorzorgsmaatregelen te nemen om het risico van een succesvolle DDoS te verminderen.
1. Analyseer verkeerspatronen gebruikend een dashboard, als uw plaats reeds levend is, om de optimale drempels voor uw regels van de de filterfilter van de tariefgrens te bepalen. Als uw site nog niet actief is, kiest u waarden op basis van uw verkeersverwachtingen.
1. Gebruikend de waarden van de vorige stap, vorm de regels van de de filterfilter van de snelheidsgrensverkeer. Zorg ervoor dat u de bijbehorende waarschuwingen inschakelt, zodat u op de hoogte wordt gesteld wanneer de drempelwaarde is bereikt.
1. Ontvang berichten van de de filterregels van het verkeersfilter wanneer de verkeerspikes voorkomen, die u van waardevol inzicht voorzien over of uw organisatie potentieel door kwaadwillige actoren wordt gericht.
1. Indien nodig handelen over de waarschuwing. Analyseer het verkeer om te bepalen als de piek wettige verzoeken eerder dan een aanval weerspiegelt. Verhoog de drempels als het verkeer wettig is, of lager hen als niet.

De rest van deze zelfstudie begeleidt u door dit proces.

## Het erkennen van de behoefte om regels te vormen {#recognize-the-need}

Zoals eerder vermeld, blokkeert de Adobe door gebrek verkeer bij CDN die een bepaald tarief overschrijdt, echter, sommige websites kunnen degraded prestaties onder die drempel ervaren. Aldus, zouden de regels van de het verkeersfilter van de tariefgrens moeten worden gevormd.

Idealiter configureert u de regels voordat u live gaat naar productie. In de praktijk, verklaren vele organisaties reactief regels slechts één keer gealarmeerd aan een verkeerspiek die op een waarschijnlijke aanval wijst.

Adobe verzendt een verkeerspiek bij oorsprongsalarm als [Melding van actiecentra](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/actions-center) wanneer een standaarddrempel van verkeer van één enkel IP adres wordt overschreden, voor bepaalde PoP. Als u zulk een alarm ontving, wordt het geadviseerd om een regel van de de verkeersfilter van de tariefgrens te vormen. Dit standaardalarm is verschillend van het alarm dat uitdrukkelijk door klanten moet worden toegelaten wanneer het bepalen van de regels van de verkeersfilter, die u over in een toekomstige sectie zult leren.


## Verkeerspatronen analyseren {#analyze-traffic}

Als uw site al actief is, kunt u de verkeerspatronen analyseren met CDN-logboeken en door Adobe verschafte dashboards.

- **CDN-verkeersdashboard**: biedt inzichten in het verkeer via CDN en de aanvraagsnelheid voor oorsprong, de foutsnelheden 4xx en 5xx en niet-in cache opgeslagen aanvragen. Verstrekt ook maximum CND en de verzoeken van de Oorsprong per seconde per cliëntIP adres en meer inzichten om de configuraties te optimaliseren CDN.

- **Hoogte-breedteverhouding CDN-cache**: biedt inzicht in de totale verhouding van cacherechthoekbelasting en het totale aantal aanvragen door HIT, PASS en MISS-status. Bevat ook URL&#39;s met de hoogste HIT-, PASS- en MISS-adressen.

De dashboardgereedschappen configureren met _een van de volgende opties_:

### ELK - dashboardgereedschappen configureren

De **Elasticsearch, Logstash en Kibana (ELK)** U kunt de CDN-logboeken analyseren met behulp van dashboardgereedschappen van Adobe. Dit tooling omvat een dashboard dat de verkeerspatronen visualiseert, makend het gemakkelijker om de optimale drempels voor uw regels van de de filterfilter van de tariefgrens te bepalen.

- Klonen met [AEMCS-CDN-Log-Analysis-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) GitHub-opslagplaats.
- De gereedschappen instellen door de opdracht [Hoe te opstelling de container van de Docker ELK](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#how-to-set-up-the-elk-docker-containerhow-to-setup-the-elk-docker-container) stappen.
- Als onderdeel van de instelling importeert u de `traffic-filter-rules-analysis-dashboard.ndjson` bestand om de gegevens te visualiseren. De _CDN-verkeer_ Het dashboard omvat visualisaties die het maximumaantal verzoeken per IP/POP bij de Rand en de Oorsprong CDN tonen.
- Van de [Cloud Manager](https://my.cloudmanager.adobe.com/)s _Omgevingen_ -kaart, downloadt u de CDN-logbestanden van de AEMCS-publicatieservice.

  ![CDN-logbestanden voor cloudbeheer](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > Het kan tot 5 minuten duren voor de nieuwe verzoeken om in de CDN- logboeken te verschijnen.

### Splunk - het vormen dashboard tooling

Klanten die [Splunk Log-doorsturen ingeschakeld](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) U kunt nieuwe dashboards tot stand brengen om de verkeerspatronen te analyseren.

Ga als volgt te werk om dashboards in Splunk te maken [Splunk dashboards voor de Analyse van het Logboek van AEMCS CDN](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis) stappen.

### Gegevens bekijken

De volgende visualisaties zijn beschikbaar in de dashboards van de Elk en van de Sprong:

- **RPS van de rand per cliënt IP en POP**: Deze visualisatie toont het maximumaantal verzoeken per IP/POP **bij de CDN-rand**. De piek in visualisatie wijst op het maximumverzoekaantal.

  **ELK-dashboard**:
  ![ELK dashboard - Maximale verzoeken per IP/POP](./assets/elk-edge-max-per-ip-pop.png)

  **Splunk Dashboard**:\
  ![Splunk dashboard - Max. verzoeken per IP/POP](./assets/splunk-edge-max-per-ip-pop.png)

- **Oorsprong RPS per client IP en POP**: Deze visualisatie toont het maximumaantal verzoeken per IP/POP **bij de oorsprong**. De piek in visualisatie wijst op het maximumverzoekaantal.

  **ELK-dashboard**:
  ![ELK dashboard - Maximale aanvragen voor oorsprong per IP/POP](./assets/elk-origin-max-per-ip-pop.png)

  **Splunk Dashboard**:
  ![Splunk dashboard - Max Origin Requests per IP/POP](./assets/splunk-origin-max-per-ip-pop.png)

## Drempelwaarden kiezen

De drempelwaarden voor de regels van de het verkeersfilter van de snelheidsgrens moeten op bovengenoemde analyse worden gebaseerd en ervoor zorgen dat het legitieme verkeer niet wordt geblokkeerd. Zie de volgende tabel voor hulp bij het kiezen van drempelwaarden:

| Variatie | Waarde |
| :--------- | :------- |
| Oorsprong | Neem de hoogste waarde van de Max Verzoeken van de Oorsprong per IP/POP onder **normaal** verkeersvoorwaarden (dat wil zeggen, niet de snelheid op het moment van een DDoS) en verhoog deze met meerdere |
| Rand | Neem de hoogste waarde van de Maximale Verzoeken van de Rand per IP/POP onder **normaal** verkeersvoorwaarden (dat wil zeggen, niet de snelheid op het moment van een DDoS) en verhoog deze met meerdere |

De te gebruiken veelvoud hangt van uw verwachtingen van normale pieken in verkeer toe te schrijven aan organisch verkeer, campagnes, en andere gebeurtenissen af. Een veelvoud tussen 5 en 10 kan redelijk zijn.

Als uw site nog niet actief is, zijn er geen gegevens om te analyseren en moet u een getrainde schatting maken van de juiste waarden die u moet instellen voor de regels voor het beperken van het verkeer. Bijvoorbeeld:

| Variatie | Waarde |
|------------------------------ |:-----------:|
| Rand | 500 |
| Oorsprong | 100 |

## Regels configureren {#configure-rules}

Vorm **snelheidslimietverkeersfilter** regels in de AEM `/config/cdn.yaml` bestand, met waarden die zijn gebaseerd op de bovenstaande beschrijving. Indien nodig, raadpleeg uw team van de Veiligheid van het Web om ervoor te zorgen de tariefgrenswaarden aangewezen zijn en geen wettig verkeer blokkeren.

Zie [Regels maken in uw AEM project](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#create-rules-in-your-aem-project) voor meer informatie .

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
    ...
    #  Prevent attack at edge by blocking client for 5 minutes if they make more than 500 requests per second on average
      - name: prevent-dos-attacks-edge
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 500 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: all # count all requests
          groupBy:
            - reqProperty: clientIp
        action: 
          type: log
          experimental_alert: true
    #  Prevent attack at origin by blocking client for 5 minutes if they make more than 100 requests per second on average            
      - name: prevent-dos-attacks-origin
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 100 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: fetches # count only fetches
          groupBy:
            - reqProperty: clientIp
        action: 
          type: log
          experimental_alert: true   
          
```

Merk op dat zowel de oorsprong als de randregels worden gedeclareerd en dat de eigenschap alert is ingesteld op `true` zodat kunt u alarm ontvangen wanneer de drempel wordt ontmoet, waarschijnlijk wijzend op een aanval.

>[!NOTE]
>
>De _experimenteel_ prefix_ voor experimentele_alert zal worden verwijderd wanneer de waakzame eigenschap wordt vrijgegeven. Als u wilt deelnemen aan het programma voor vroege adoptie, stuurt u een e-mail naar **<aemcs-waf-adopter@adobe.com>**.

Men adviseert dat het handelingstype aan logboek aanvankelijk wordt geplaatst zodat kunt u verkeer voor een paar uren of dagen controleren, ervoor zorgen dat het wettige verkeer deze tarieven niet overschrijdt. Schakel na een paar dagen over naar de blokmodus.

Voer de volgende stappen uit om de wijzigingen in uw AEMCS-omgeving te implementeren:

- Leg de bovenstaande wijzigingen vast en duw deze naar de gegevensopslagruimte van Cloud Manager.
- Implementeer de wijzigingen in de AEMCS-omgeving met behulp van de Config-pijplijn van Cloud Manager. Vernieuwen [Regels implementeren via Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) voor meer informatie .
- Om te verifiëren **regel van de snelheidsbeperking van het verkeersfilter** werkt zoals verwacht, kunt u een aanval simuleren zoals in [Aanvalsimulatie](#attack-simulation) sectie. Beperk het aantal aanvragen tot een waarde die hoger is dan de grenswaarde voor het tarief die in de regel is ingesteld.

### Transformatieregels voor aanvragen configureren {#configure-request-transform-rules}

Naast de regels van de snelheidsgrensfilter, wordt het geadviseerd om te gebruiken [aanvraagtransformaties](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#request-transformations) om vraagparameters ongedaan te maken niet nodig door de toepassing om manieren te minimaliseren om het geheime voorgeheugen door geheim voorgeheugensteunende technieken te mijden. Als u bijvoorbeeld alleen wilt toestaan `search` en `campaignId` queryparameters, kan de volgende regel worden gedeclareerd:

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: 
    - dev
    - stage
    - prod  
data:  
  experimental_requestTransformations:
    rules:            
      - name: unset-all-query-params-except-those-needed
        when:
          reqProperty: tier
          in: ["publish"]
        actions:
          - type: unset
            queryParamMatch: ^(?!search$|campaignId$).*$
```

## Waarschuwingen over verkeersfilterregels ontvangen {#receiving-alerts}

Zoals hierboven vermeld, als de verkeersfilterregel *trial_alert: true*, wordt een alarm ontvangen wanneer de regel wordt aangepast.

## Actie inzake signaleringen {#acting-on-alerts}

Soms is de waarschuwing informatief, wat je een idee geeft van de frequentie van aanvallen. Het is nuttig om uw CDN gegevens te analyseren gebruikend het dashboard hierboven beschreven, om te bevestigen dat de verkeerspiek aan een aanval, en niet alleen een verhoging van wettig verkeersvolume is toe te schrijven. Als dat het geval is, kunt u de drempelwaarde verhogen.

## Aanvalsimulatie{#attack-simulation}

Deze sectie beschrijft methodes om een aanval van Dos te simuleren, die kan worden gebruikt om gegevens voor de dashboards te produceren die in dit leerprogramma worden gebruikt en te bevestigen dat om het even welke gevormde regels met succes aanvallen blokkeren.

>[!CAUTION]
>
> Voer deze stappen niet uit in een productieomgeving. De volgende stappen dienen alleen voor simulatiedoeleinden.
> 
>Als u een alarm hebt ontvangen die op een piek in verkeer wijst, ga aan te werk [Verkeerspatronen analyseren](#analyzing-traffic-patterns) sectie.

Om een aanval te simuleren, hulpmiddelen zoals [Apache Benchmark](https://httpd.apache.org/docs/2.4/programs/ab.html), [Apache JMeter](https://jmeter.apache.org/), [Vegeta](https://github.com/tsenart/vegeta)en andere kunnen worden gebruikt.

### Edge-verzoeken

Het volgende gebruiken [Vegeta](https://github.com/tsenart/vegeta) kunt u veel aanvragen indienen bij uw website:

```shell
$ echo "GET https://<YOUR-WEBSITE-DOMAIN>" | vegeta attack -rate=120 -duration=5s | vegeta report
```

Het bovenstaande bevel maakt 120 verzoeken om 5 seconden en output een rapport. Ervan uitgaande dat de snelheid van de website niet beperkt is, kan dit tot een toename van het verkeer leiden.

### Oorsprongsverzoeken

Als u de CDN-cache wilt overslaan en aanvragen bij de oorsprong wilt indienen (AEM publicatieservice), kunt u een unieke queryparameter aan de URL toevoegen. Raadpleeg het voorbeeldscript van Apache JMeter in het dialoogvenster [DoS-aanval simuleren met JMeter-script](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection#simulate-dos-attack-using-jmeter-script)

