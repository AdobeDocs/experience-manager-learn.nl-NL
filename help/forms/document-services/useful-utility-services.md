---
title: Nuttige nutsvoorzieningen
description: Een aantal nuttige hulpservices voor AEM Forms-ontwikkelaars
feature: Adaptieve Forms
version: 6.4,6.5
topic: Ontwikkeling
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 1%

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

De voorbeeldbundel kan [hier worden gedownload](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Voorbeeldcode voor het gebruik van de hulpprogrammaservice(s)

Hier volgt de code die in de JSP-pagina is gebruikt om org.w3c.dom.Document te maken van een tekenreeks en het document om te zetten en op te slaan in de CRX-opslagruimte, zoals in het volgende codefragment wordt getoond.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Vereisten


U moet [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) implementeren en de bundel starten.


Als u documenten wilt opslaan in de CRX-opslagruimte met deze hulpprogrammaservice, volgt u [ontwikkelen met het artikel van de servicegebruiker](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Zorg ervoor u [vereiste toestemmingen ](http://localhost:4502/useradmin) op de aangewezen omslagen CRX aan de fd-dienst gebruiker verstrekt.

