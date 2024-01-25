---
title: Een servlet registreren met een brontype
description: Een servlet toewijzen aan een brontype in AEM Forms CS
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
duration: 110
exl-id: 2a33a9a9-1eef-425d-aec5-465030ee9b74
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# Inleiding

Het binden van servlets door wegen heeft verscheidene nadelen wanneer vergeleken bij het binden door middeltypes, namelijk:

* De verbindende servers van de weg kunnen niet worden gecontroleerd gebruikend standaardJCR bewaarplaats ACLs
* Padgebonden servlets kunnen slechts aan een weg en niet een middeltype worden geregistreerd (d.w.z. geen achtervoegselbehandeling)
* Als een verbindende servlet niet actief is, bijvoorbeeld als de bundel ontbreekt of niet begonnen is, zou een POST in onverwachte resultaten kunnen resulteren. gewoonlijk een knooppunt maken bij `/bin/xyz` die vervolgens het pad van de servlets bindt dat de toewijzing bindt, niet transparant is voor een ontwikkelaar die alleen naar de repository kijkt Gezien deze nadelen wordt het ten zeerste aanbevolen om servlets te binden aan middeltypes in plaats van paden

## Servlet maken

Start je aem-banking project in IntelliJ. Maak een servlet met de naam GetFieldChoices onder de servlets-map, zoals hieronder in de schermafbeelding wordt weergegeven.
![keuzen](assets/fetchchoices.png)

## Sample Servlet

Het volgende servlet is gebonden aan het het Verdraaien middeltype: _**azuur/keuzes**_



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

* Meld u aan bij de lokale AEM SDK.
* Een naam voor een bron maken `fetchchoices` (u kunt dit knooppunt toch een naam geven) van het type `cq:Page` onder inhoudsknooppunt.
* Uw wijzigingen opslaan
* Een knooppunt maken met de naam `jcr:content` van het type `cq:PageContent` en sla de wijzigingen op
* Voeg de volgende eigenschappen toe aan de `jcr:content` node

| Eigenschapnaam | Waarde van eigenschap |
|--------------------|--------------------|
| jcr:titel | Utility Servlets |
| sling:resourceType | `azure/fetchchoices` |


De `sling:resourceType` value must match resourceTypes=&quot;azure/fetchchoice gespecificeerd in servlet.

U kunt uw servlet nu aanhalen door de bron aan te vragen met `sling:resourceType` = `azure/fetchchoices` op zijn volledige weg, met om het even welke selecteurs of uitbreidingen die in het Sling servlet worden geregistreerd.

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

Het pad `/content/fetchchoices/jcr:content` is de weg van het middel en de uitbreiding `.json` is wat in servlet wordt gespecificeerd

## Uw AEM synchroniseren

1. Open het AEM project in uw favoriete redacteur. Daar heb ik intelliJ voor gebruikt.
1. Een map maken met de naam `fetchchoices` krachtens `\aem-banking-application\ui.content\src\main\content\jcr_root\content`
1. Klikken met rechtermuisknop `fetchchoices` map en selecteer `repo | Get Command` (Dit menu-item is ingesteld in een vorig hoofdstuk van deze zelfstudie).

Dit knooppunt moet worden gesynchroniseerd van AEM naar uw lokale AEM.

Uw AEM projectstructuur moet er als volgt uitzien
![resource-resolver](assets/mapping-servlet-resource.png)
Filter.xml bijwerken in de map aem-banking-application\ui.content\src\main\content\META-INF\vault met de volgende vermelding

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

U kunt uw wijzigingen nu uitvoeren naar een AEM as a Cloud Service omgeving met gebruik van Cloud Manager.

## Volgende stappen

[Forms Portal-componenten inschakelen](./forms-portal-components.md)
