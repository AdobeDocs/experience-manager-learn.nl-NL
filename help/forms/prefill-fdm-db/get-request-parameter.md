---
title: Request-parameter ophalen
description: Toegang tot de verzoekparameter tot de prefill dienst van een model van vormgegevens
feature: Adaptieve Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Ontwikkeling
role: Developer
level: Begin
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 1%

---

# Request-parameter ophalen

## Get empID, parameter

De volgende stap bestaat uit het benaderen van de empID-parameter vanaf de URL. De waarde van de empID-aanvraagparameter wordt vervolgens doorgegeven aan de servicebewerking **_get_** van het formuliergegevensmodel.
Met het oog op deze cursus hebben we het volgende gemaakt en verstrekt

* Aangepast formuliersjabloon met de naam **_FDMDemo_**
* Paginacomponent **_fdmdemo_**
* De aangepaste JPEG-indeling is opgenomen in de paginacomponent
* De adaptieve formuliersjabloon is gekoppeld aan de paginacomponent

Op deze manier wordt onze code in de aangepaste jsp alleen uitgevoerd wanneer het adaptieve formulier dat is gebaseerd op deze aangepaste sjabloon, wordt weergegeven

* [Het ](assets/template-page-component.zip) pakket importeren met  [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* [fdmrequest.jsp openen](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Verwijder de commentaarregel.
* Uw wijzigingen opslaan

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

De waarde van empID is gekoppeld aan de sleutel empID in paraMap. Deze kaart wordt vervolgens doorgegeven aan de slingRequest

>[!NOTE]
>
>De belangrijkste empID moet met de bindende waarde van de naburige entiteiten de dienst krijgen
