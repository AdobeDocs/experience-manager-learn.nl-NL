---
title: Aangepaste naamruimten
description: Leer hoe u aangepaste naamruimten definieert en implementeert in AEM as a Cloud Service.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
duration: 496
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# Aangepaste naamruimten

Leer hoe te om douane [ namespaces ](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) aan AEM as a Cloud Service te bepalen en op te stellen.

Aangepaste naamruimten zijn het optionele deel van een JCR-eigenschap dat voorafgaat aan een `:` . AEM gebruikt verschillende naamruimten, zoals:

+ `jcr` voor JCR-systeemeigenschappen
+ `cq` voor AEM (voorheen Adobe CQ genoemd) eigenschappen
+ `dam` voor AEM eigenschappen die specifiek zijn voor DAM-elementen
+ `dc` voor Dublin Core-eigenschappen

... en vele anderen.

Naamruimten kunnen worden gebruikt om het bereik en de intentie van een eigenschap aan te geven. Het creëren van een douanespatie, vaak uw bedrijfsnaam, helpt duidelijk knopen of eigenschappen identificeren specifiek voor uw AEM implementatie en gegevens bevatten specifiek voor uw zaken.

De douane namespaces wordt beheerd in [ het Schipen van de Initialisatie van de Bewaarplaats (opnieuw richt) ](https://sling.apache.org/documentation/bundles/repository-initialization.html) manuscripten, en stelt aan AEM as a Cloud Service als configuraties OSGi op - en toegevoegd aan het ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) `ui.config` project van uw [ AEM project.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## Bronnen

+ [ het Sling Repository Initialization (repoinit) documentatie ](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Code

De volgende code wordt gebruikt om een naamruimte `wknd` te configureren.

### Configuratie RepositoryInitializer OSGi

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Hierdoor kunnen aangepaste eigenschappen die de naamruimte `wknd` gebruiken, zoals de eerste parameter na de instructie `register namespace` , in AEM worden gebruikt. Voor geavanceerdere manuscriptdefinities, herzie de voorbeelden in de [ Verschuivende Documentatie van de Initialisatie van de Bewaarplaats van de Bewaarplaats (repoinit) ](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
