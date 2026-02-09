---
title: Verouderde API's zoeken en verwijderen in AEM as a Cloud Service
description: Leer hoe u verouderde API's kunt zoeken en verwijderen in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
role: Developer, Architect
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20288
thumbnail: KT-20288.png
last-substantial-update: 2026-02-09T00:00:00Z
source-git-commit: 6c5b911d1d59573338dd1a30eb95289bc1339f19
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 0%

---


# Verouderde API&#39;s zoeken en verwijderen in AEM as a Cloud Service

Leer hoe u verouderde API&#39;s kunt zoeken en verwijderen in AEM as a Cloud Service.

## Overzicht

AEM as a Cloud Service **Centrum van de Actie** brengt u over _afgekeurde APIs_ in uw project op de hoogte. Om de nieuwste functies, beveiligingsupdates en vloeiende implementaties van uw code naar AEM as a Cloud Service te krijgen via Cloud Manager-pijpleidingen, verwijdert u verouderde API&#39;s uit uw project.

In dit leerprogramma, leert u hoe te om verouderde APIs in uw milieu van AEM as a Cloud Service te vinden en te verwijderen gebruikend de [ Analysator Maven Insteekmodule van AEM ](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

## Vervangen API&#39;s zoeken

Voer de volgende stappen uit om verouderde API&#39;s te zoeken in uw AEM as a Cloud Service-project.

1. **Gebruik de recentste Insteekmodule van de Analysator van AEM Gemaakt**

   In uw project van AEM, gebruik de recentste versie van de [ Gebruikte Insteekmodule van de Analysator van AEM ](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

   - In de hoofdmap `pom.xml` wordt de versie van de plug-in meestal gedeclareerd. Vergelijk uw versie met de recentste [ vrijgegeven versie ](https://mvnrepository.com/artifact/com.adobe.aem/aemanalyser-maven-plugin).

     ```xml
     ...
     <aemanalyser.version>1.6.14</aemanalyser.version> <!-- Latest released version as of 09-Feb-2026 -->
     ...
     <!-- AEM Analyser Plugin -->
     <plugin>
         <groupId>com.adobe.aem</groupId>
         <artifactId>aemanalyser-maven-plugin</artifactId>
         <version>${aemanalyser.version}</version>
         <extensions>true</extensions>
     </plugin>
     ...
     ```

   - De insteekmodule controleert op de nieuwste beschikbare AEM SDK. Gebruik de nieuwste AEM SDK-versie in het `pom.xml` -bestand van uw project. Het helpt om verouderde APIs als waarschuwingen van winde te behandelen.

     ```xml
     ...
     <aem.sdk.api>2026.2.24288.20260204T121510Z-260100</aem.sdk.api> <!-- Latest available AEM SDK version as of 09-Feb-2026 -->
     ...
     ```

   - Controleer of de plug-in van de module `all` in de `verify` -fase wordt uitgevoerd.

     ```xml
     ...
     <build>
         <plugins>
             ...
             <plugin>
                 <groupId>com.adobe.aem</groupId>
                 <artifactId>aemanalyser-maven-plugin</artifactId>
                 <extensions>true</extensions>
                 <executions>
                     <execution>
                         <id>analyse-project</id>
                         <phase>verify</phase>
                         <goals>
                             <goal>project-analyse</goal>
                         </goals>
                     </execution>
                 </executions>
             </plugin>
             ...
         </plugins>
     </build>
     ...
     ```

2. **stel een bouwstijl in werking en controleer waarschuwingen**

   Wanneer u `mvn clean install` in werking stelt, verouderde de analysator APIs als **[WAARSCHUWING]** berichten in de output. Bijvoorbeeld:

   ```shell
   ...
   [WARNING] The analyser found the following warnings for author and publish :
   [WARNING] [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
   ...
   ```

   Het is gemakkelijk om deze berichten over te zien wanneer het concentreren zich op bouwstijlsucces of mislukking.

3. **krijg een duidelijke lijst van afgekeurde APIs**

   Deze stap bevat ook dezelfde informatie. Nochtans, stel de `verify` fase op de `all` module in werking om alle **[WAARSCHUWENDE]** berichten op één plaats te zien. Bijvoorbeeld:

   ```shell
   $ mvn clean verify -pl all
   ```

   **[WAARSCHUWING]** berichten in de bouwstijloutput maakt een lijst van afgekeurde APIs in uw project.

## Verouderde API&#39;s verwijderen

De Analysator van AEM rapporteert **wat** wordt afgekeurd en verstrekt de **aanbeveling** op hoe te om het te bevestigen. Gebruik echter de onderstaande tabel om de juiste actie te kiezen en volg de gekoppelde documentatie wanneer u meer details nodig hebt.

### Verouderde API-herstelstrategie

| Waarschuwingstype Analyzer | Wat het aangeeft | Aanbevolen actie | Referentie |
| --------------------- | ----------------- | ------------------ | --------- |
| Verouderde AEM API | API moet uit AEM as a Cloud Service worden verwijderd | Gebruik vervangen door de ondersteunde openbare API | [ API de Begeleiding van de Verwijdering ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Vervangen AEM-pakket of -klasse | Pakket of klasse wordt niet meer ondersteund | Refactorcode om het aanbevolen alternatief te gebruiken | [ Vervangen APIs ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#aem-apis) |
| Vervangen bibliotheek van derden | Bibliotheek wordt niet ondersteund in toekomstige SDK&#39;s | Verbetering van het gebruik van afhankelijkheid en refactor | [ Algemene Richtlijnen ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Vervangen Sling/OSGi-patronen | Oudere annotaties of API&#39;s gedetecteerd | Migreren naar moderne Sling- en OSGi-API&#39;s | [ Verwijdering van de Patronen Sling/OSGi ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Geplande verwijdering (datum in de toekomst) | API werkt nog wel, maar verwijdering wordt later afgedwongen | Opschonen van plannen vóór handhaving van de pijpleiding | [ de nota&#39;s van de Versie ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/home) |

### Praktische aanwijzingen

- Behandel analyseprogrammawaarschuwingen als **toekomstige pijpleidingsmislukkingen**, niet facultatieve berichten.
- Vervangen APIs plaatselijk gebruikend **recentste AEM SDK** verhelpen.
- Houd de uitvoer van de analysator schoon om problemen tijdens toekomstige AEM-upgrades te voorkomen.

Het bevestigen afgekeurde APIs houdt vroeg uw project **verbetering-veilig en plaatsing-klaar**.

## Aanvullende bronnen

- [ AEM Analyser Maven Insteekmodule ](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)
- [ Vervangen en Verwijderde Eigenschappen en APIs ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance)

