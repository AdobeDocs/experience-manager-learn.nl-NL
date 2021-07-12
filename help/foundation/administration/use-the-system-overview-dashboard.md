---
title: Het dashboard Systeemoverzicht in AEM gebruiken
description: In vorige versies van AEM beheerders moesten verschillende locaties bekijken om een volledig beeld van de AEM te krijgen. Het overzicht van het Systeem streeft ernaar dit op te lossen door een mening op hoog niveau van de configuratie, de hardware, en de gezondheid van de AEM instantie allen van één enkel dashboard te verstrekken.
version: 6.4, 6.5
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
topic: Beheer
role: Admin
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---


# Het dashboard Systeemoverzicht gebruiken

Adobe Experience Manager (AEM) [!UICONTROL System Overview] verstrekt een mening op hoog niveau van de configuratie, de hardware, en de gezondheid van de AEM instantie allen van één enkel dashboard.

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. Het Overzicht van het Systeem kan van worden betreden: **AEM Start** > **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL System Overview]**

   Direct om **`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. De informatie van [!UICONTROL System Overview] kan worden uitgevoerd door [!UICONTROL Download] knoop te klikken. De informatie wordt ook via het volgende [!DNL REST] eindpunt blootgesteld:
1. Hieronder ziet u een voorbeelduitvoer van de JSON die wordt geëxporteerd uit de [!UICONTROL System Overview]:

   ```json
   {
       "Health Checks": {
           "1 Critical": "System Maintenance",
           "3 Warn": "Replication Queue, Log Errors, Sling/Granite Content Access Check"
       },
       "Instance": {
           "Adobe Experience Manager": "6.4.0",
           "Run Modes": "s7connect, crx3, non-composite, author, samplecontent, crx3tar",
           "Instance Up Since": "2018-01-22 10:50:37"
       },
       "Repository": {
           "Apache Jackrabbit Oak": "1.8.0",
           "Node Store": "Segment Tar",
           "Repository Size": "0.26 GB",
           "File Data Store": "crx-quickstart/repository/datastore"
       },
       "Maintenance Tasks": {
           "Failed": "AuditLog Maintenance Task, Project Purge, Workflow Purge",
           "Succeeded": "Data Store Garbage Collection, Lucene Binaries Cleanup, Revision Clean Up, Version Purge, Purge of ad-hoc tasks"
       },
       "System Information": {
           "Mac OS X": "10.12.6",
           "System Load Average": "2.29",
           "Usable Disk Space": "89.44 GB",
           "Maximum Heap": "0.97 GB"
       },
       "Estimated Node Counts": {
           "Total": "232448",
           "Tags": "62",
           "Assets": "267",
           "Authorizables": "210",
           "Pages": "1592"
       },
       "Replication Agents": {
           "Blocked": "publish, publish2",
           "Idle": "offloading_b3deb296-9b28-4f60-8587-c06afa2e632c, offloading_outbox, offloading_reverse_b3deb296-9b28-4f60-8587-c06afa2e632c, publish_reverse, scene7, screens, screens2, test_and_target"
       },
       "Distribution Agents": {
           "Blocked": "publish"
       },
       "Workflows": {
           "Running Workflows": "15",
           "Failed Workflows": "15",
           "Failed Jobs": "150"
       },
       "Sling Jobs": {
           "Failed": "305"
       }
   }
   ```
