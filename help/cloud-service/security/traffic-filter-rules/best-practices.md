---
title: Aanbevolen werkwijzen voor verkeersfilterregels, inclusief WAF-regels
description: Leer geadviseerde beste praktijken voor de regels van de Filter van het Verkeer met inbegrip van de regels van WAF.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
duration: 170
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# Beste praktijken voor de regels van het verkeersfilter met inbegrip van de regels van WAF

Leer geadviseerde beste praktijken voor de regels van de verkeersfilter, met inbegrip van de regels van WAF. Het is belangrijk om op te merken dat de beste praktijken die in dit artikel worden beschreven niet uitputtend zijn en niet bedoeld zijn om uw eigen veiligheidsbeleid en procedures te vervangen.

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## Algemene beste praktijken

- Om te bepalen welke regels voor uw organisatie aangewezen zijn, werk met uw veiligheidsteam samen.
- Test altijd regels in Dev-omgevingen voordat u ze implementeert in werkgebied- en productieomgevingen.
- Beginnen bij het declareren en valideren van regels altijd met `action` type `log` om ervoor te zorgen dat de regel geen legitiem verkeer blokkeert.
- Voor bepaalde regels geldt de overgang van `log` tot `block` dient uitsluitend gebaseerd te zijn op een analyse van voldoende verkeer ter plaatse.
- Introduceer regels incrementeel, en overweeg het betrekken van uw testteams (QA, prestaties, penetratietests) in het proces.
- De impact van regels regelmatig analyseren met behulp van de [dashboardgereedschap](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool). Afhankelijk van het verkeersvolume van uw site kan de analyse dagelijks, wekelijks of maandelijks worden uitgevoerd.
- Om kwaadwillig verkeer te blokkeren dat u zich na de analyse kunt bewust zijn, voeg om het even welke extra regels toe. Bijvoorbeeld, bepaalde IPs die uw plaats hebben aangevallen.
- Het creëren, de plaatsing, en de analyse van de regel zouden een lopend, herhalend proces moeten zijn. Het is geen eenmalige activiteit.

## Beste werkwijzen voor verkeersfilterregels

Laat de hieronder regels van de verkeersfilter voor uw AEM project toe. De gewenste waarden voor `rateLimit` en `clientCountry` moeten worden bepaald in samenwerking met uw beveiligingsteam.

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
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block
    # Block requests coming from OFAC countries
      - name: block-ofac-countries
        when:
          allOf:
              - reqProperty: tier
              - matches: publish
              - reqProperty: clientCountry
                in:
                  - SY
                  - BY
                  - MM
                  - KP
                  - IQ
                  - CD
                  - SD
                  - IR
                  - LR
                  - ZW
                  - CU
                  - CI
```

>[!WARNING]
>
>Voor uw productiemilieu, werk met uw team van de Veiligheid van het Web samen om de aangewezen waarden voor te bepalen `rateLimit`

## Aanbevolen procedures voor WAF-regels

Zodra WAF vergunning en toegelaten voor uw programma wordt gegeven, verschijnen de vlaggen van WAF van het verkeer passende in grafieken en verzoeklogboeken, zelfs als u hen niet in een regel verklaarde. Dit is zodat bent u zich altijd bewust van potentieel nieuw kwaadwillig verkeer en kan regels tot stand brengen zoals nodig. Bekijk de vlaggen van WAF die niet in de verklaarde regels worden weerspiegeld en denk na verklarend hen.

Overweeg de WAF regels hieronder voor uw AEM project. De gewenste waarden voor `action` en `wafFlags` moet worden bepaald in samenwerking met uw beveiligingsteam.

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

    # Traffic Filter rules shown in above section
    ...

    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
    # Disable protection against CMDEXE on /bin
      - name: allow-cdmexe-on-root-bin
        when:
          allOf:
            - reqProperty: tier
              matches: "author|publish"
            - reqProperty: path
              matches: "^/bin/.*"
        action:
          type: allow
          wafFlags:
            - CMDEXE
```

## Samenvatting

Tot slot beschikt deze zelfstudie over de kennis en gereedschappen die nodig zijn om de beveiliging van uw webtoepassingen in Adobe Experience Manager as a Cloud Service (AEMCS) te versterken. Met praktische regelvoorbeelden en inzichten in resultaatanalyse kunt u uw website en toepassingen effectief beschermen.



