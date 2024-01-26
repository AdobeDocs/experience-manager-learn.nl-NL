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
duration: 357
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 0%

---

# Werken met AEM oudere Cloud Servicen

In deze sectie bespreken we hoe we Adobe Experience Manager (AEM) met Adobe Target kunnen instellen met behulp van verouderde Cloud Servicen.

>[!NOTE]
>
> De AEM Legacy Cloud Service met Adobe Target is **alleen** gebruikt om directe AEMauteur aan Adobe Target achterste-eindverbinding tot stand te brengen die de publicatie van inhoud van AEM aan Doel vergemakkelijkt. De Lancering van de Adobe wordt gebruikt blootstelt Adobe Target op het publiek onder ogen ziende Web-site ervaring die door AEM wordt gediend.

Als u gebruik wilt maken van AEM Experience Fragment-aanbiedingen om uw personalisatieactiviteiten te stimuleren, kunt u verdergaan met het volgende hoofdstuk en AEM integreren met Adobe Target met behulp van de verouderde cloudservices. Deze integratie is vereist om Experience Fragments van AEM naar Target te duwen als HTML/JSON aanbiedingen en om de doelaanbiedingen gelijk te houden met AEM. Deze integratie is vereist voor de implementatie [Scenario 1 besproken in de overzichtssectie](./overview.md#personalization-using-aem-experience-fragment).

## Vereisten

* **AEM**

   * AEM auteur- en publicatieexemplaar zijn nodig om deze zelfstudie te voltooien. Als u nog geen AEM hebt ingesteld, kunt u de stappen volgen [hier](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud voorzien van de volgende oplossingen
      * [Adobe Target](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > De klant moet van Experience Platform Launch en Adobe I/O van worden voorzien [Ondersteuning voor Adoben](https://helpx.adobe.com/nl/contact/enterprise-support.ec.html) of reik uit uw systeembeheerder

### AEM integreren met Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Adobe Target-Cloud Service maken met Adobe IMS-verificatie (*Gebruikt Adobe Target API*(00:34)
2. Adobe Target-clientcode verkrijgen (01:50)
3. Adobe IMS-configuratie voor Adobe Target maken (02:08)
4. Een technische account maken voor toegang tot doel-API in Adobe I/O Console (02:08)
5. Adobe Target-Cloud Service toevoegen aan AEM Experience Fragments (04:12)

U hebt nu de [AEM met Adobe Target met behulp van verouderde Cloud Servicen](./using-aem-cloud-services.md#integrating-aem-target-options) zoals beschreven in Optie 2. U moet nu een Experience Fragment kunnen maken binnen AEM en het Experience Fragment kunnen publiceren als HTML-aanbieding of JSON-aanbieding aan Adobe Target, en vervolgens kunnen worden gebruikt om een activiteit te maken.
