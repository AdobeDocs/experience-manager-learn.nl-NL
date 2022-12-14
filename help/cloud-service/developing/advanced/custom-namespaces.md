---
title: Aangepaste naamruimten
description: Leer hoe u aangepaste naamruimten definieert en implementeert om as a Cloud Service te AEM.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 79804ac1ec18f8ac4815bd5ee6f309ef85110cb9
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Aangepaste naamruimten

Leer hoe u aangepaste toepassingen definieert en implementeert [naamruimten](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) AEM as a Cloud Service.

Aangepaste naamruimten zijn het optionele deel van een JCR-eigenschap dat voorafgaat aan een `:`. AEM gebruikt verschillende naamruimten, zoals:

+ `jcr` voor eigenschappen van JCR-systemen
+ `cq` voor AEM (voorheen Adobe CQ genoemd) eigenschappen
+ `dam` voor AEM eigenschappen die specifiek zijn voor DAM-middelen
+ `dc` voor Dublin Core-eigenschappen

... en vele anderen.

Naamruimten kunnen worden gebruikt om het bereik en de intentie van een eigenschap aan te geven. Het creëren van een douanespatie, vaak uw bedrijfsnaam, helpt duidelijk knopen of eigenschappen identificeren specifiek voor uw AEM implementatie en gegevens bevatten specifiek voor uw zaken.

Aangepaste naamruimten worden beheerd in [Initialisatie van opslagplaats verkopen (opnieuw aanwijzen)](https://sling.apache.org/documentation/bundles/repository-initialization.html) manuscripten, en stelt aan AEM as a Cloud Service als configuraties OSGi op - en toegevoegd aan uw [AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) `ui.config` project.

>[!VIDEO](https://video.tv.adobe.com/v/3412319/?quality=12&learn=on)

## Bronnen

+ [Initialisatiedocumentatie (repoint-it) voor Sling Repository](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Code

De volgende code wordt gebruikt om een `wknd` naamruimte.

### Configuratie RepositoryInitializer OSGi

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Hierdoor kunnen aangepaste eigenschappen `wknd` naamruimte, zoals wordt aangegeven als de eerste parameter na de `register namespace` instructie, te gebruiken in AEM. Voor geavanceerdere scriptdefinities raadpleegt u de voorbeelden in het dialoogvenster [Initialisatiedocumentatie (repoint-it) voor Sling Repository](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
