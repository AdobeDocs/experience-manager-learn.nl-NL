---
title: Aanbevolen werkwijzen voor verkeersfilterregels, inclusief WAF-regels
description: Leer geadviseerde beste praktijken voor de regels van de Filter van het Verkeer met inbegrip van de regels van WAF.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 0%

---

# Aanbevolen werkwijzen voor verkeersfilterregels, inclusief WAF-regels

Leer geadviseerde beste praktijken voor de regels van de verkeersfilter, met inbegrip van de regels van WAF. Het is belangrijk om op te merken dat de beste praktijken die in dit artikel worden beschreven niet uitputtend zijn en niet bedoeld zijn om uw eigen veiligheidsbeleid en procedures te vervangen.

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## Algemene beste praktijken

- Om te bepalen welke regels voor uw organisatie aangewezen zijn, werk met uw veiligheidsteam samen.
- Test altijd regels in Dev-omgevingen voordat u ze implementeert in werkgebied- en productieomgevingen.
- Wanneer u regels declareert en valideert, moet u altijd beginnen met `action` type `log` om te zorgen dat de regel het legitieme verkeer niet blokkeert.
- Voor bepaalde regels moet de overgang van `log` naar `block` uitsluitend gebaseerd zijn op een analyse van voldoende siteverkeer.
- Introduceer regels incrementeel, en overweeg het betrekken van uw testteams (QA, prestaties, penetratietests) in het proces.
- Analyseer regelmatig het effect van regels gebruikend het [ dashboard tooling ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). Afhankelijk van het verkeersvolume van uw site kan de analyse dagelijks, wekelijks of maandelijks worden uitgevoerd.
- Om kwaadwillig verkeer te blokkeren dat u zich na de analyse kunt bewust zijn, voeg om het even welke extra regels toe. Bijvoorbeeld, bepaalde IPs die uw plaats hebben aangevallen.
- Het creëren, de plaatsing, en de analyse van de regel zouden een lopend, herhalend proces moeten zijn. Het is geen eenmalige activiteit.

## Beste werkwijzen voor verkeersfilterregels

Schakel de onderstaande regels voor verkeersfilters in voor uw AEM-project. De gewenste waarden voor de eigenschappen `rateLimit` en `clientCountry` moeten echter worden bepaald in samenwerking met uw beveiligingsteam.

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
>Voor uw productieomgeving werkt u samen met uw webbeveiligingsteam om de juiste waarden voor `rateLimit` te bepalen

## Aanbevolen procedures voor WAF-regels

Zodra de WAF vergunning en toegelaten voor uw programma is, verkeer die de vlaggen van WAF aanpassen verschijnen in grafieken en verzoeklogboeken, zelfs als u hen niet in een regel verklaarde. Zo, bent u zich altijd bewust van potentieel nieuw kwaadwillig verkeer en kan regels tot stand brengen zoals nodig. Kijk naar WAF-vlaggen die niet worden weerspiegeld in de gedeclareerde regels en denk eraan ze te declareren.

Houd rekening met de onderstaande WAF-regels voor uw AEM-project. De gewenste waarden voor de eigenschappen `action` en `wafFlags` moeten echter worden bepaald in samenwerking met uw beveiligingsteam.

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



