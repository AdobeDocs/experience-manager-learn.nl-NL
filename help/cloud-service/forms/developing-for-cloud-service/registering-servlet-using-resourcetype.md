---
title: Een servlet registreren met een brontype
description: Een servlet toewijzen aan een brontype in AEM Forms CS
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
duration: 90
exl-id: 2a33a9a9-1eef-425d-aec5-465030ee9b74
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# Inleiding

Het binden van servlets door wegen heeft verscheidene nadelen wanneer vergeleken bij het binden door middeltypes, namelijk:

* De verbindende servers van de weg kunnen niet worden gecontroleerd gebruikend standaardJCR bewaarplaats ACLs
* Padgebonden servlets kunnen slechts aan een weg en niet een middeltype worden geregistreerd (d.w.z. geen achtervoegselbehandeling)
* Als een verbindende servlet niet actief is, bijvoorbeeld als de bundel ontbreekt of niet begonnen is, zou een POST in onverwachte resultaten kunnen resulteren. meestal een knooppunt bij `/bin/xyz` maken dat vervolgens de padbinding van de servlets bedekt
de toewijzing is niet transparant voor een ontwikkelaar die alleen naar de opslagplaats kijkt
Gezien deze nadelen wordt het sterk geadviseerd om servers aan middeltypes eerder dan wegen te binden

## Servlet maken

Start je aem-banking project in IntelliJ. Maak een servlet met de naam GetFieldChoices onder de servlets-map, zoals hieronder in de schermafbeelding wordt weergegeven.
![&#x200B; keuzen &#x200B;](assets/fetchchoices.png)

## Sample Servlet

De volgende servlet is verbindend aan het het Verspreiden middeltype: _&#x200B;**azure/fetchchoice**&#x200B;_



```java
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import javax.servlet.Servlet;
import java.io.IOException;
import java.io.Serializable;

@Component(
        service={Servlet.class }
)

        @SlingServletResourceTypes(
                resourceTypes="azure/fetchchoices",
                methods= "GET",
                extensions="json"
                )


public class GetFieldChoices extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final  transient Logger log = LoggerFactory.getLogger(this.getClass());


   

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {

        log.debug("The form path I got was "+request.getParameter("formPath"));

    }
}
```

## Bronnen maken in CRX

* Meld u aan bij uw lokale AEM SDK.
* Maak een bron met de naam `fetchchoices` (u kunt dit knooppunt desgewenst een naam geven) van het type `cq:Page` onder het inhoudsknooppunt.
* Uw wijzigingen opslaan
* Maak een knooppunt met de naam `jcr:content` type `cq:PageContent` en sla de wijzigingen op
* Voeg de volgende eigenschappen toe aan het knooppunt `jcr:content`

| Eigenschapnaam | Waarde van eigenschap |
|--------------------|--------------------|
| jcr:titel | Utility Servlets |
| sling:resourceType | `azure/fetchchoices` |


De `sling:resourceType` waarde moet resourceTypes=&quot;azure/fetchchoice aanpassen gespecificeerd in servlet.

U kunt nu uw servlet aanroepen door de bron met `sling:resourceType` = `azure/fetchchoices` aan te vragen bij het volledige pad, met alle kiezers of extensies die zijn geregistreerd in het Sling-servlet.

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

Het pad `/content/fetchchoices/jcr:content` is het pad van de bron en de extensie `.json` is wat is opgegeven in de servlet

## AEM-project synchroniseren

1. Open het AEM-project in uw favoriete editor. Daar heb ik intelliJ voor gebruikt.
1. Een map maken met de naam `fetchchoices` onder `\aem-banking-application\ui.content\src\main\content\jcr_root\content`
1. Klik met de rechtermuisknop op de map `fetchchoices` en selecteer `repo | Get Command` (Dit menu-item is ingesteld in een vorig hoofdstuk van deze zelfstudie).

Dit knooppunt moet worden gesynchroniseerd van AEM naar uw lokale AEM-project.

Uw AEM-projectstructuur moet er als volgt uitzien
![&#x200B; middel-resolver &#x200B;](assets/mapping-servlet-resource.png)
Filter.xml bijwerken in de map aem-banking-application\ui.content\src\main\content\META-INF\vault met de volgende vermelding

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

U kunt nu met Cloud Manager uw wijzigingen in een AEM as a Cloud Service-omgeving doorvoeren.

## Volgende stappen

[Forms Portal-componenten inschakelen](./forms-portal-components.md)
