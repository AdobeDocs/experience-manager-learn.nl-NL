---
title: Adobe Experience Manager met Adobe Target integreren met behulp van Cloud Servicen
description: Stap voor stap doorloopt u hoe u Adobe Experience Manager (AEM) met Adobe Target kunt integreren met AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
duration: 337
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 0%

---

# Werken met AEM oudere Cloud Servicen

In deze sectie bespreken we hoe we Adobe Experience Manager (AEM) met Adobe Target kunnen instellen met behulp van verouderde Cloud Servicen.

>[!NOTE]
>
> De AEM Cloud Service van de Oudheid met Adobe Target wordt **slechts** gebruikt om directe AEMAuteur aan Adobe Target achterste eindverbinding te vestigen die de publicatie van inhoud van AEM aan Doel vergemakkelijkt. De markeringen in Adobe Experience Platform worden gebruikt stellen Adobe Target op het publiek onder ogen ziende Web-site ervaring bloot die door AEM wordt gediend.

Als u gebruik wilt maken van AEM Experience Fragment-aanbiedingen om uw personalisatieactiviteiten te stimuleren, kunt u verdergaan met het volgende hoofdstuk en AEM integreren met Adobe Target met behulp van de verouderde cloudservices. Deze integratie is vereist om Experience Fragments van AEM naar Target te duwen als HTML/JSON aanbiedingen en om de doelaanbiedingen gelijk te houden met AEM. Deze integratie wordt vereist voor het uitvoeren van [ Scenario 1 die in de overzichtssectie ](./overview.md#personalization-using-aem-experience-fragment) wordt besproken.

## Vereisten

* **AEM**

   * AEM auteur- en publicatieexemplaar zijn nodig om deze zelfstudie te voltooien. Als u opstelling uw AEM instantie nog niet hebt, kunt u de stappen [ hier ](./implementation.md#set-up-aem) volgen.

* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud voorzien van de volgende oplossingen
      * [ Adobe Target ](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > De klant moet met de Inzameling en Adobe I/O van Gegevens van [ steun van de Adobe ](https://helpx.adobe.com/nl/contact/enterprise-support.ec.html) worden voorzien of uit reiken aan uw systeembeheerder

### AEM integreren met Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Creeer de Cloud Service van Adobe Target gebruikend de Authentificatie van Adobe IMS (*gebruikt Adobe Target API*) (00:34)
2. Adobe Target-clientcode verkrijgen (01:50)
3. Adobe IMS-configuratie voor Adobe Target maken (02:08)
4. Een technische account maken voor toegang tot doel-API in Adobe I/O Console (02:08)
5. Adobe Target-Cloud Service toevoegen aan AEM Experience Fragments (04:12)

Op dit punt, hebt u met succes [ AEM met Adobe Target geïntegreerd gebruikend Verouderde Cloud Servicen ](./using-aem-cloud-services.md#integrating-aem-target-options) zoals die in Optie 2 worden gedetailleerd. U moet nu een Experience Fragment kunnen maken binnen AEM en het Experience Fragment kunnen publiceren als HTML-aanbieding of JSON-aanbieding aan Adobe Target, en vervolgens kunnen worden gebruikt om een activiteit te maken.
