---
title: Request-parameter ophalen
description: Toegang tot de verzoekparameter tot de prefill dienst van een model van vormgegevens
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
duration: 40
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 0%

---

# Request-parameter ophalen

## Get empID, parameter

De volgende stap bestaat uit het benaderen van de empID-parameter vanaf de URL. De waarde van empID verzoekparameter wordt dan overgegaan tot **_krijgt_** de dienstverrichting van het model van vormgegevens.
Met het oog op deze cursus hebben we het volgende gemaakt en verstrekt

* Het adaptieve vormmalplaatje riep **_FDMDemo_**
* De Component van de pagina riep **_fdmdemo_**
* De aangepaste JPEG-indeling is opgenomen in de paginacomponent
* De adaptieve formuliersjabloon is gekoppeld aan de pagina-component

Op deze manier wordt onze code in de aangepaste jsp alleen uitgevoerd wanneer het adaptieve formulier dat is gebaseerd op deze aangepaste sjabloon, wordt weergegeven

* [ de Invoer het pakket ](assets/template-page-component.zip) gebruikend [ pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)
* [ Open fdmrequest.jsp ](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
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

## Volgende stappen

[Een adaptief formulier maken op basis van een formuliergegevensmodel](./create-adaptive-form.md)
