---
title: Een taak uitvoeren op een leader-instantie in AEM as a Cloud Service
description: Leer hoe u een taak uitvoert op de leader-instantie in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
topic: Development
feature: OSGI, Cloud Manager
role: Architect, Developer
level: Intermediate, Experienced
doc-type: Article
duration: 0
last-substantial-update: 2024-10-23T00:00:00Z
jira: KT-16399
thumbnail: KT-16399.jpeg
exl-id: b8b88fc1-1de1-4b5e-8c65-d94fcfffc5a5
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---

# Een taak uitvoeren op een leader-instantie in AEM as a Cloud Service

Leer hoe u een taak uitvoert op de instantie leader in de AEM Author-service als onderdeel van AEM as a Cloud Service en hoe u deze zo configureert dat deze slechts eenmaal wordt uitgevoerd.

Het verkopen van Banen zijn asynchrone taken die op de achtergrond werken, die worden ontworpen om systeem of gebruiker-teweeggebrachte gebeurtenissen te behandelen. Deze taken worden standaard gelijkmatig over alle instanties (pods) in de cluster verdeeld.

Voor meer informatie, zie [ Apache die Gebeurtenis en de Behandeling van de Baan &lbrace;](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html) schikt.

## Taken maken en verwerken

Voor demodoeleinden, creeer een eenvoudige _baan die de baanbewerker opdraagt om een bericht_ te registreren.

### Een taak maken

Gebruik de hieronder code om __ tot een Apache het Schillen baan te leiden:

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import java.util.HashMap;
import java.util.Map;

import org.apache.sling.event.jobs.JobManager;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(immediate = true)
public class SimpleJobCreaterImpl {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobCreaterImpl.class);

    // Define the topic on which the job will be created
    protected static final String TOPIC = "wknd/simple/job/topic";

    // Inject a JobManager
    @Reference
    private JobManager jobManager;

    @Activate
    protected final void activate() throws Exception {
        log.info("SimpleJobCreater activated successfully");
        createJob();
        log.info("SimpleJobCreater created a job");
    }

    private void createJob() {
        // Create a job and add it on the above defined topic
        Map<String, Object> jobProperties = new HashMap<>();
        jobProperties.put("action", "log");
        jobProperties.put("message", "Job metadata is: Created in activate method");
        jobManager.addJob(TOPIC, jobProperties);
    }
}
```

De belangrijkste punten in de bovenstaande code zijn:

- De taaklading heeft twee eigenschappen: `action` en `message` .
- Gebruikend de [ methode van JobManager ](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/apache/sling/event/jobs/JobManager.html) `addJob(...)`, wordt de baan toegevoegd aan het onderwerp `wknd/simple/job/topic`.

### Een taak verwerken

Gebruik de hieronder code aan _proces_ de bovengenoemde baan van het Sllen Apache:

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import org.apache.sling.event.jobs.Job;
import org.apache.sling.event.jobs.consumer.JobConsumer;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = JobConsumer.class, property = {
        JobConsumer.PROPERTY_TOPICS + "=" + SimpleJobCreaterImpl.TOPIC
}, immediate = true)
public class SimpleJobConsumerImpl implements JobConsumer {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobConsumerImpl.class);

    @Override
    public JobResult process(Job job) {
        // Get the action and message properties
        String action = job.getProperty("action", String.class);
        String message = job.getProperty("message", String.class);

        // Log the message
        if ("log".equals(action)) {
            log.info("Processing WKND Job, and {}", message);
        }

        // Return a successful result
        return JobResult.OK;
    }

}
```

De belangrijkste punten in de bovenstaande code zijn:

- De klasse `SimpleJobConsumerImpl` implementeert de interface `JobConsumer` .
- Het is een service die is geregistreerd om taken uit het onderwerp `wknd/simple/job/topic` te gebruiken.
- De methode `process(...)` verwerkt de taak door de eigenschap `message` van de taaklading te registreren.

### Standaardtaakverwerking

Wanneer u de bovenstaande code implementeert in een AEM as a Cloud Service-omgeving en deze uitvoert op de AEM Author-service, die fungeert als een cluster met meerdere JVM&#39;s van AEM Author, wordt de taak één keer uitgevoerd op elke AEM Author-instantie (pod). Dit betekent dat het aantal gemaakte taken overeenkomt met het aantal pods. Het aantal pods zal altijd meer dan één zijn (voor niet-RDE-omgevingen), maar zal fluctueren op basis van het interne bronnenbeheer van AEM as a Cloud Service.

De taak wordt uitgevoerd op elke instantie van AEM Auteur (pod) omdat de `wknd/simple/job/topic` is gekoppeld aan de hoofdwachtrij van AEM, die taken over alle beschikbare instanties verdeelt.

Dit is vaak problematisch als de baan voor het veranderen van staat, zoals het creëren van of het bijwerken van middelen of externe diensten verantwoordelijk is.

Als u de baan slechts één keer op de dienst van de Auteur van AEM wilt lopen, voeg de [ configuratie van de baanrij ](#how-to-run-a-job-on-the-leader-instance) toe hieronder beschreven.

U kunt het verifiëren door de logboeken van de dienst van de Auteur van AEM in [ Cloud Manager ](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs#cloud-manager) te herzien.

![ Baan die door alle instanties wordt verwerkt ](./assets/run-job-once/job-processed-by-all-instances.png)


U zou moeten zien:

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-nxxcx] *INFO* [sling-oak-observation-15] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method

<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-r4zk7] *INFO* [sling-oak-observation-11] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```

Er zijn twee logitems, één voor elke AEM Auteur-instantie (`68775db964-nxxcx` en `68775db964-r4zk7` ), die aangeven dat elke instantie (pod) de taak heeft verwerkt.

## Een taak uitvoeren op de leaderinstantie

Om een baan _in werking te stellen slechts eenmaal_ op de dienst van de Auteur van AEM, creeer een nieuwe het Verdelen baanrij van het type **&#x200B;**&#x200B;wordt bevolen, en associeer uw baanonderwerp (`wknd/simple/job/topic`) met deze rij. Met deze configuratie kan alleen de AEM Author-instantie (pod) voor leader de taak verwerken.

Maak in de module `ui.config` van uw AEM-project een OSGi-configuratiebestand ( `org.apache.sling.event.jobs.QueueConfiguration~wknd.cfg.json` ) en sla dit op in de map `ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author` .

```json
{
    "queue.name":"WKND Queue - ORDERED",
    "queue.topics":[
      "wknd/simple/job/topic"
    ],
    "queue.type":"ORDERED",
    "queue.retries":1,
    "queue.maxparallel":1.0
  }
```

De belangrijkste punten om in de bovenstaande configuratie te nota te nemen zijn:

- Het onderwerp van de wachtrij is ingesteld op `wknd/simple/job/topic` .
- Het type wachtrij is ingesteld op `ORDERED` .
- Het maximumaantal parallelle taken is ingesteld op `1` .

Nadat u de bovenstaande configuratie hebt geïmplementeerd, wordt de taak uitsluitend door de instantie leader verwerkt, zodat deze maar één keer door de hele AEM Author-service wordt uitgevoerd.

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-7475cf85df-qdbq5] *INFO* [FelixLogListener] Events.Service.org.apache.sling.event Service [QueueMBean for queue WKND Queue - ORDERED,7755, [org.apache.sling.event.jobs.jmx.StatisticsMBean]] ServiceEvent REGISTERED
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
<DD.MM.YYYY HH:mm:ss.SSS> [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```
