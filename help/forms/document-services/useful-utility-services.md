---
title: Nuttige nutsvoorzieningen
description: Een aantal nuttige hulpservices voor AEM Forms-ontwikkelaars
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
duration: 35
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Nuttige nutsvoorzieningen

Deze voorbeeldbundel biedt nuttige hulpservices die door een AEM Forms-ontwikkelaar kunnen worden gebruikt. De volgende services zijn beschikbaar.


```java
package aemformsutilityfunctions.core;
import java.util.Map;
import com.adobe.aemfd.docmanager.Document;
public interface AemFormsUtilities
{
public abstract com.adobe.aemfd.docmanager.Document createDDXFromMapOfDocuments(Map<String, com.adobe.aemfd.docmanager.Document> paramMap);
public abstract org.w3c.dom.Document w3cDocumentFromStrng(String xmlString);
public abstract com.adobe.aemfd.docmanager.Document orgw3cDocumentToAEMFDDocument(org.w3c.dom.Document xmlDocument);
public abstract String saveDocumentInCrx(String jcrPath,String fileExtension, Document documentToSave);

}
```

De steekproefbundel kan [ van hier worden gedownload ](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Voorbeeldcode voor het gebruik van de hulpprogrammaservice(s)

Hier volgt de code die in de JSP-pagina is gebruikt om org.w3c.dom.Document te maken op basis van een tekenreeks en het document om te zetten en op te slaan in de CRX-opslagruimte, zoals in het volgende codefragment wordt getoond.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Vereisten


U zult [ DevelopingWithServiceUserBundle ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) moeten opstellen en de bundel beginnen.


Als u documenten in de bewaarplaats van CRX gebruikend deze nutsdienst gaat bewaren, te volgen gelieve het [ ontwikkelen met het artikel van de de dienstgebruiker ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Zorg ervoor u de [ vereiste toestemmingen ](http://localhost:4502/useradmin) op de aangewezen omslagen van CRX aan de fd-dienst gebruiker verstrekt.
