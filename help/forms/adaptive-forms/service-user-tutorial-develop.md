---
title: Ontwikkelen met servicegebruikers in AEM Forms
seo-title: Ontwikkelen met servicegebruikers in AEM Forms
description: In dit artikel wordt u door het proces geleid voor het maken van een servicegebruiker in AEM Forms
seo-description: In dit artikel wordt u door het proces geleid voor het maken van een servicegebruiker in AEM Forms
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: adaptive-forms
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---


# Ontwikkelen met servicegebruikers in AEM Forms

In dit artikel wordt u door het proces geleid voor het maken van een servicegebruiker in AEM Forms

In vorige versies van Adobe Experience Manager (AEM) werd de beheerresourceoplosser gebruikt voor back-end verwerking, waarvoor toegang tot de opslagplaats was vereist. Het gebruik van de oplossing van de administratieve bron is afgekeurd in AEM 6.3. In plaats daarvan wordt een systeemgebruiker met specifieke machtigingen in de opslagplaats gebruikt.

Dit artikel doorloopt de verwezenlijking van een systeemgebruiker en het vormen van de eigenschappen van de gebruikerskaart.

1. Navigeer naar [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Aanmelden als beheerder
1. Klik op Gebruikersbeheer
1. Klik op Systeemgebruiker maken
1. Stel het type gebruikersnaam in als &#39; data &#39; en klik op het groene pictogram om het maken van de systeemgebruiker te voltooien
1. [Open configMgr](http://localhost:4502/system/console/configMgr)
1. Zoek naar &#39;&#39;Apache Sling Service User Mapper Service&#39; en klik om de eigenschappen te openen
1. Klik op het pictogram *+* (plus) om de volgende servicetoewijzing toe te voegen

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Klik op Opslaan &#39;

In de bovenstaande configuratie-instelling DevelopingWithServiceUser.core is de symbolische naam van de bundel. getresourceresolver is subservice name.data is de systeemgebruiker die in de vroegere stap wordt gecreeerd.

We kunnen ook resourceresolver krijgen namens gebruikers van fd-service. Deze servicegebruiker wordt gebruikt voor documentservices. Als u bijvoorbeeld gebruiksrechten wilt certificeren/toepassen enz., gebruiken we resourceoploser van gebruikers van fd-service om de bewerkingen uit te voeren

1. [Download en decomprimeer het ZIP-bestand dat aan dit artikel is gekoppeld.](assets/developingwithserviceuser.zip)
1. Navigeer naar [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Upload en start de OSGi-bundel
1. Zorg ervoor dat de bundel actief is
1. U hebt nu met succes een *Gebruiker van het Systeem* gecreeerd en ook de *bundel van de Gebruiker van de Dienst* opgesteld.

   Om toegang tot /content te verlenen, geef de systeemgebruiker (&#39; data &#39;) toestemmingen op de inhoudsknoop lezen.

   1. Navigeer naar [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Zoeken naar gegevens van gebruiker &#39;. Dit is dezelfde systeemgebruiker die u in de vorige stap hebt gemaakt.
   1. Dubbelklik op de gebruiker en klik vervolgens op het tabblad &#39;Machtigingen&#39;
   1. &#39; read &#39; toegang geven tot de map &#39;content&#39;.
   1. Om de de dienstgebruiker te gebruiken om toegang tot /content omslag te verkrijgen gebruik de volgende code

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   Als u toegang wilt krijgen tot het bestand /content/dam/data.json in uw bundel, gebruikt u de volgende code. Deze code veronderstelt u gelezen toestemmingen aan de &quot;gegevens&quot;gebruiker op /content/dam/ knoop hebt gegeven

   ```java
   @Reference
   GetResolver getResolver;
   .
   .
   .
   ResourceResolver serviceResolver = getResolver.getServiceResolver();
   // to get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();
   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
   ```

   De volledige code van de uitvoering wordt hieronder gegeven

   ```java
   package com.mergeandfuse.getserviceuserresolver.impl;
   
   import java.util.HashMap;
   
   import org.apache.sling.api.resource.LoginException;
   import org.apache.sling.api.resource.ResourceResolver;
   import org.apache.sling.api.resource.ResourceResolverFactory;
   import org.osgi.service.component.annotations.Component;
   import org.osgi.service.component.annotations.Reference;
   import com.mergeandfuse.getserviceuserresolver.GetResolver;
   
   @Component(service = GetResolver.class)
   public class GetResolverImpl implements GetResolver {
    @Reference
    ResourceResolverFactory resolverFactory;
    @Override
    public ResourceResolver getServiceResolver() {
     HashMap<String, Object> param = new HashMap<String, Object>();
     param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
     ResourceResolver resolver = null;
     try {
      resolver = resolverFactory.getServiceResourceResolver(param);
     } catch (LoginException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
     }
     return resolver;
    }
   ```

