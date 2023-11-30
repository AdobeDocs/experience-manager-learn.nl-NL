---
title: Contextbewuste configuratieoverschrijvingsondersteuning voor formuliergegevensmodel
description: Stel formuliergegevensmodellen in om te communiceren met verschillende eindpunten op basis van omgevingen.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 0%

---

# Contextbewuste cloudconfiguraties

Wanneer u wolkenconfiguratie in uw lokale milieu en bij succesvol het testen creeert zou u de zelfde wolkenconfiguratie in uw stroomopwaartse milieu&#39;s willen gebruiken maar zonder het moeten het eindpunt, geheime sleutel/wachtwoord en of gebruikersbenaming veranderen. Om dit gebruiksgeval te bereiken, heeft AEM Forms on Cloud Service de mogelijkheid geïntroduceerd om context-bewuste wolkenconfiguraties te definiëren.
De cloudconfiguratie van de Azure-opslagaccount kan bijvoorbeeld opnieuw worden gebruikt in ontwikkelings-, werkgebied- en productieomgevingen met behulp van verschillende verbindingstekenreeksen en -sleutels.

De volgende stappen zijn nodig om een contextbewuste cloudconfiguratie te maken

## Omgevingsvariabelen maken

Standaardomgevingsvariabelen kunnen worden geconfigureerd en beheerd via Cloud Manager. Zij worden verstrekt aan het runtime milieu en kunnen in configuraties worden gebruikt OSGi. [Omgevingsvariabelen kunnen milieuspecifieke waarden of omgevingsgeheimen zijn, op basis van wat wordt gewijzigd.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)



De volgende schermafbeelding toont de omgevingsvariabelen azure_key en azure_connection_string die zijn gedefinieerd
![environment_variables](assets/environment-variables.png)

Deze omgevingsvariabelen kunnen vervolgens worden opgegeven in de configuratiebestanden die in de juiste omgeving moeten worden gebruikt. Als u bijvoorbeeld wilt dat alle auteur-instanties deze omgevingsvariabelen gebruiken, definieert u het configuratiebestand in de map config.auther hieronder.

## Configuratiebestand maken

Open uw project in IntelliJ. Navigeer naar config.auteur en creeer een dossier genoemd

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

Kopieer de volgende tekst naar het bestand dat u in de vorige stap hebt gemaakt. De code in dit bestand overschrijft de waarde van de eigenschappen accountName en accountKey met de omgevingsvariabelen **azure_connection_string** en **azure_key**.

```json
{
  "enabled":true,
  "description":"dermisITOverrideConfig",
  "overrides":[
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountName=\"$[env:azure_connection_string]\"",
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountKey=\"$[secret:azure_key]\""

  ]
}
```

>[!NOTE]
>
>Deze configuratie is van toepassing op alle auteursomgevingen in uw cloudservice-instantie. Om de configuratie toe te passen om milieu&#39;s te publiceren zult u het zelfde configuratiedossier in config.publish omslag van uw intelliJ project moeten plaatsen
>[!NOTE]
> Controleer of de eigenschap die wordt overschreven een geldige eigenschap van de cloudconfiguratie is. Navigeer naar de wolkenconfiguratie om het bezit te vinden dat u zoals hieronder getoond wilt met voeten treden.

![cloud-config-eigenschap](assets/cloud-config-properties.png)

Voor op REST gebaseerde cloudconfiguratie met basisauthentificatie zult u typisch omgevingsvariabelen voor serviceEndPoint, userName, en wachtwoordeigenschappen willen tot stand brengen.

## Volgende stappen

[Uw AEM-project naar cloudmanager overbrengen](./push-project-to-cloud-manager-git.md)
