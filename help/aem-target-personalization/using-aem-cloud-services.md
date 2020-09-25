---
title: Adobe Experience Manager integreren met Adobe Target met behulp van Cloud Services
seo-title: Adobe Experience Manager (AEM) integreren met Adobe Target met behulp van verouderde Cloud Services
description: Stap voor stap doorloopt u hoe u Adobe Experience Manager (AEM) met Adobe Target kunt integreren met AEM Cloud Service
seo-description: Stap voor stap doorloopt u hoe u Adobe Experience Manager (AEM) met Adobe Target kunt integreren met AEM Cloud Service
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 1%

---


# Werken met AEM oudere Cloud Services

In deze sectie bespreken we hoe we Adobe Experience Manager (AEM) met Adobe Target kunnen instellen met behulp van verouderde Cloud Services.

>[!NOTE]
>
> De AEM Legacy Cloud Service met Adobe Target wordt **alleen** gebruikt om een directe back-end verbinding van AEM-auteur met Adobe Target tot stand te brengen die de publicatie van inhoud van AEM naar Target vergemakkelijkt. De Lancering van Adobe wordt gebruikt blootstelt Adobe Target op het publiek onder ogen ziende Web-site ervaring die door AEM wordt gediend.

Als u gebruik wilt maken van AEM Experience Fragment-aanbiedingen om uw personalisatieactiviteiten te stimuleren, kunt u verdergaan met het volgende hoofdstuk en AEM integreren met Adobe Target met behulp van de verouderde cloudservices. Deze integratie is vereist om Experience Fragments van AEM naar Target te duwen als HTML/JSON-aanbiedingen en om de doelaanbiedingen gelijk te houden met AEM. Deze integratie is vereist voor de implementatie van [scenario 1 dat in de overzichtssectie](./overview.md#personalization-using-aem-experience-fragment)wordt besproken.

## Vereisten

* **AEM**

   * AEM auteur- en publicatieexemplaar zijn nodig om deze zelfstudie te voltooien. Als u nog geen AEM hebt ingesteld, kunt u de stappen [hier](./implementation.md#set-up-aem)volgen.

* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Experience Cloud voorzien van de volgende oplossingen
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > De klant moet van Experience Platform Launch en Adobe I/O van de steun [van de](https://helpx.adobe.com/nl/contact/enterprise-support.ec.html) Adobe voorzien of uw systeembeheerder bereiken



### AEM integreren met Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Adobe Target-Cloud Service maken met Adobe IMS-verificatie (*gebruikt Adobe Target API*) (00:34)
2. Adobe Target-clientcode verkrijgen (01:50)
3. Adobe IMS-configuratie maken voor Adobe Target (02:08)
4. Een technische account maken voor toegang tot doel-API in Adobe I/O-console (02:08)
5. Adobe Target-Cloud Service toevoegen aan AEM Experience Fragments (04:12)

U hebt nu [AEM met Adobe Target geïntegreerd met behulp van verouderde Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options) , zoals beschreven in Optie 2. U moet nu een Experience Fragment kunnen maken binnen AEM en het Experience Fragment kunnen publiceren als HTML-aanbieding of JSON-aanbieding aan Adobe Target, en vervolgens kunnen worden gebruikt om een activiteit te maken.
