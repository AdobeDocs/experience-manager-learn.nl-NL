---
title: Adobe Experience Manager integreren met Adobe Target met behulp van Cloud Services
seo-title: Integrating Adobe Experience Manager (AEM) with Adobe Target using Legacy Cloud Services
description: Stap voor stap doorloopt u hoe u Adobe Experience Manager (AEM) met Adobe Target kunt integreren met AEM Cloud Service
seo-description: Step by step walkthrough on how to integrate Adobe Experience Manager (AEM) with Adobe Target using AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---


# Werken met AEM oudere Cloud Services

In deze sectie bespreken we hoe we Adobe Experience Manager (AEM) met Adobe Target kunnen instellen met behulp van verouderde Cloud Services.

>[!NOTE]
>
> De AEM Oudere Cloud Service met Adobe Target is **only** gebruikt om directe AEM Auteur aan Adobe Target achterste-eindverbinding tot stand te brengen die de publicatie van inhoud van AEM aan Doel vergemakkelijkt. De Lancering van Adobe wordt gebruikt blootstelt Adobe Target op het publiek onder ogen ziende Web-site ervaring die door AEM wordt gediend.

Als u gebruik wilt maken van AEM Experience Fragment-aanbiedingen om uw personalisatieactiviteiten te stimuleren, kunt u verdergaan met het volgende hoofdstuk en AEM integreren met Adobe Target met behulp van de verouderde cloudservices. Deze integratie is vereist om Experience Fragments van AEM naar Target te duwen als HTML/JSON-aanbiedingen en om de doelaanbiedingen gelijk te houden met AEM. Deze integratie is vereist voor het uitvoeren van [Scenario 1 die in overzichtssectie](./overview.md#personalization-using-aem-experience-fragment) wordt besproken.

## Vereisten

* **AEM**

   * AEM auteur- en publicatieexemplaar zijn nodig om deze zelfstudie te voltooien. Als u nog geen opstelling uw AEM instantie hebt, kunt u de stappen [hier ](./implementation.md#set-up-aem) volgen.

* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud voorzien van de volgende oplossingen
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > De klant moet van [Adobe steun](https://helpx.adobe.com/nl/contact/enterprise-support.ec.html) voorzien van Experience Platform Launch en Adobe I/O of bereik aan uw systeembeheerder


### AEM integreren met Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Adobe Target-Cloud Service maken met Adobe IMS-verificatie (*Gebruikt Adobe Target API*) (00:34)
2. Adobe Target-clientcode verkrijgen (01:50)
3. Adobe IMS-configuratie maken voor Adobe Target (02:08)
4. Een technische account maken voor toegang tot doel-API in Adobe I/O Console (02:08)
5. Adobe Target-Cloud Service toevoegen aan AEM Experience Fragments (04:12)

Op dit punt hebt u [AEM met Adobe Target geïntegreerd met behulp van verouderde Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options), zoals beschreven in Optie 2. U moet nu een Experience Fragment kunnen maken binnen AEM en het Experience Fragment kunnen publiceren als HTML-aanbieding of JSON-aanbieding aan Adobe Target, en vervolgens kunnen worden gebruikt om een activiteit te maken.
