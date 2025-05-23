---
title: Vul Adaptieve Forms met behulp van queryparameters.
description: Vul Adaptieve Forms met gegevens van queryparameters.
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-11470
last-substantial-update: 2020-11-12T00:00:00Z
exl-id: 14ac6ff9-36b4-415e-a878-1b01ff9d3888
duration: 49
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Adaptieve Forms vooraf vullen met queryparameters

Een van onze klanten had de vereiste om adaptief formulier in te vullen met behulp van de queryparameters. In de volgende URL worden bijvoorbeeld de velden FirstName en LastName in het adaptieve formulier ingesteld op respectievelijk Jan en Doe

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

Hiervoor is een nieuwe aangepaste formuliersjabloon gemaakt en gekoppeld aan een pagina-component. In deze paginacomponent hebben wij jsp om greep van de vraagparameters te krijgen en een xml structuur tot stand te brengen die kan worden gebruikt om de adaptieve vorm te bevolken.

De details bij het creëren van een nieuw Adaptief malplaatje van de Vorm en paginacomponent worden [ verklaard in deze video.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=nl-NL)

Hier volgt de code die is gebruikt op de JSP-pagina

```java
java.util.Enumeration enumeration = request.getParameterNames();
String dataXml = "<afData><afUnboundData><data>";
while (enumeration.hasMoreElements())
{
   String parameterName = (String) enumeration.nextElement();
   dataXml = dataXml + "<" + parameterName + ">" + request.getParameter(parameterName) + "</" + parameterName + ">";

}

dataXml = dataXml + "</data></afUnboundData></afData>";
//System.out.println("The data xml is "+dataXml);
slingRequest.setAttribute("data", dataXml);
```

>[!NOTE]
>
>Als uw formulier een schema gebruikt, is de structuur van uw xml anders en moet u de xml overeenkomstig samenstellen.


## De elementen op uw systeem implementeren

* [Download en installeer de adaptieve formuliersjabloon met Package Manager](assets/populate-with-xml.zip)
* [Download en installeer het voorbeeldadaptieve formulier](assets/populate-af-with-query-paramters-form.zip)

* [ Voorproef de adaptieve vorm ](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
Het adaptieve formulier moet worden gevuld met de waarde Jan en Smit
