---
title: AEM Author Service caching
description: Algemeen overzicht van het in cache plaatsen van AEM as a Cloud Service-auteurservice.
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: b8e09820-f1f2-4897-b454-16c0df5a0459
duration: 56
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---

# AEM-auteur

AEM Author heeft caching beperkt wegens de hoogst dynamische, en toestemmingsgevoelige aard van de inhoud het dient. Over het algemeen wordt het niet aangeraden caching aan te passen voor AEM Author en in plaats daarvan te vertrouwen op de cacheconfiguraties die door Adobe worden geleverd om een prestatiebeleving te garanderen.

![ AEM Auteur caching overzichtsdiagram ](./assets/author/author-all.png){align="center"}

Hoewel het aanpassen van caching op de Auteur van AEM wordt ontmoedigd, is het nuttig om te begrijpen dat de Auteur van AEM een Adobe-beheerde CDN heeft, maar geen AEM Dispatcher heeft. Houd er rekening mee dat alle AEM Dispatcher-configuraties op AEM Author worden genegeerd, omdat deze geen AEM Dispatcher hebben.

## CDN

De dienst van de Auteur van AEM gebruikt CDN, nochtans is zijn doel de levering van productmiddelen te verbeteren, en zou niet uitgebreid moeten worden gevormd, in plaats daarvan het laten werken zoals het is.

![ AEM publiceert het caching overzichtsdiagram ](./assets/author/author-cdn.png){align="center"}

De AEM-auteur-CDN bevindt zich tussen de eindgebruiker, meestal een markteur of auteur van inhoud, en de AEM-auteur. Er worden onveranderlijke bestanden in het cachegeheugen opgeslagen, zoals statische elementen die de AEM-ontwerpervaring versterken, en niet geschreven inhoud.

CDN van de Auteur van AEM leidt verscheidene types van middelen in het voorgeheugen op die van belang kunnen zijn, met inbegrip van a [ klantgerichte TTL op Verlengde Vragen ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=nl-NL&author-instances), en a [ lange TTL op de Bibliotheken van de douaneCliënt ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#client-side-libraries).

### Standaardcache-levensduur

De volgende klant die middelen onder ogen ziet worden in het voorgeheugen ondergebracht door de Auteur CDN van AEM, en hebben het volgende standaardgeheim voorgeheugenleven:

| Inhoudstype | Standaardlevensduur van CDN-cache |
|:------------ |:---------- |
| [ Blijven vragen (JSON) ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=nl-NL&author-instances) | 1 minuut |
| [ de bibliotheken van de Cliënt (JS/CSS) ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#client-side-libraries) | dertig dagen |
| [ Al het andere ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#other-content) | Niet in cache geplaatst |


## AEM Dispatcher

De dienst van de Auteur van AEM omvat AEM Dispatcher niet, en gebruikt slechts [ CDN ](#cdn) voor caching.
