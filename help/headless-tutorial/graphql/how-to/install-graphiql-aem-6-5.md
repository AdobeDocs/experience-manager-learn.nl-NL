---
title: GraphiQL IDE installeren op AEM 6.5.X
description: Leer hoe u de GraphiQL IDE op AEM 6.5.X-versie installeert en configureert
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
source-git-commit: ae27cbc50fc5c4c2e8215d7946887b99d480d668
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 3%

---


# GraphiQL IDE installeren op AEM 6.5.X

In AEM 6.5 moet het GraphiQL IDE hulpmiddel manueel worden geïnstalleerd, volg hieronder stappen voor installatie en configuraties.

1. Ga naar de **[Software Distribution Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**.
1. Zoek naar &quot;GraphiQL&quot; (zorg ervoor dat u de **i** in **GraphiQL**.
1. Download de nieuwste **GraphiQL Content Package v.x.x.x**

   ![GraphiQL-pakket downloaden](assets/graphiql/software-distribution.png)

   Het ZIP-bestand is een AEM pakket dat rechtstreeks kan worden geïnstalleerd.

1. Navigeer in het menu AEM Start naar **Gereedschappen** > **Implementatie** > **Pakketten**.
1. Klikken **Pakket uploaden** en kiest u het pakket dat u in de vorige stap hebt gedownload. Klikken **Installeren** om het pakket te installeren.

   ![GraphiQL-pakket installeren](assets/graphiql/install-graphiql-package.png)

1. Navigeren naar **CRXDE Lite** > **Deelvenster Opslagplaats** > selecteren `/content/graphiql` node (bijvoorbeeld <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. In de **Eigenschappen** tabwijzigingswaarde van `endpoint` eigenschap aan `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Waarde van eindpunteigenschap wijzigen](assets/graphiql/endpoint-prop-value-change.png)

1. Ga naar de **Webconsoleconfiguratie** UI > Zoeken naar **CSRF-filter** configuratie (bijvoorbeeld<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. In de `Excluded Paths` eigenschap naam veld update, WKND GraphQL eindpuntpad naar `/content/cq:graphql/wknd-shared/endpoint`.
   ![Waarde van padeigenschap uitsluiten](assets/graphiql/exclude-paths-value-change.png)

1. Heb toegang tot de redacteur GraphiQL gebruikend `//HOST:PORT/content/graphiql.html`en verifieer u kunt een nieuwe query samenstellen of een bestaande query uitvoeren. (bijv. <http://localhost:4502/content/graphiql.html>)

![GraphiQL Editor](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Als u uw projectspecifieke GraphQL-schema en query-uitvoering wilt ondersteunen, moet u de bijbehorende wijzigingen aanbrengen voor het `endpoint` en `Excluded Paths` waarden in bovenstaande stappen.