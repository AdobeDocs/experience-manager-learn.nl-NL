---
title: GraphiQL IDE installeren op AEM 6.5
description: Leer hoe te installeren en te vormen GraphiQL winde op AEM 6.5
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# GraphiQL IDE installeren op AEM 6.5

In AEM 6.5 moet het hulpmiddel GraphiQL IDE manueel worden geïnstalleerd.

1. Navigeer aan het **[Portaal van de Distributie van de Software ](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**.
1. Onderzoek naar &quot;GraphiQL&quot;(ben zeker om **i** in **GraphiQL** te omvatten).
1. Download het recentste **GrahiQL Pakket van de Inhoud v.x.x.x.x**.

   ![ Pakket GraphiQL van de Download {](assets/graphiql/software-distribution.png)

   Het ZIP-bestand is een AEM pakket dat rechtstreeks kan worden geïnstalleerd.

1. Van het AEM menu van het Begin, navigeer aan **Hulpmiddelen** > **Plaatsing** > **Pakketten**.
1. Klik **Upload Pakket** en kies het pakket in de vroegere stap wordt gedownload dat. Klik **installeren** om het pakket te installeren.

   ![ installeer GrahiQL Pakket ](assets/graphiql/install-graphiql-package.png)

1. Navigeer aan **CRXDE Lite** > **het Comité van de Bewaarplaats** > selecteert `/content/graphiql` knoop (bijvoorbeeld, <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. In het **bezit van Eigenschappen** lusje verandert waarde van `endpoint` bezit in `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![ Verandering van de Waarde van het Eindpuntbezit ](assets/graphiql/endpoint-prop-value-change.png)

1. Navigeer aan de **Configuratie van de Console van het Web** UI > Onderzoek naar **CSRF de configuratie van de Filter** (bijvoorbeeld, <http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. In de `Excluded Paths` update van het gebied van de bezitsnaam, het WKND GraphQL eindpuntweg aan `/content/cq:graphql/wknd-shared/endpoint`.

![ sluit de Verandering van de Waarde van het Bezit van Wegen ](assets/graphiql/exclude-paths-value-change.png) uit

1. Heb toegang tot de redacteur GraphiQL gebruikend `//HOST:PORT/content/graphiql.html`, en verifieer u een nieuwe vraag kunt construeren of een bestaande vraag uitvoeren. (bijvoorbeeld <http://localhost:4502/content/graphiql.html>)

![ GraphiQL Redacteur ](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Als u uw projectspecifieke GraphQL-schema en query-uitvoering wilt ondersteunen, moet u de overeenkomende wijzigingen voor de `endpoint` - en `Excluded Paths` -waarden in de bovenstaande stappen doorvoeren.
