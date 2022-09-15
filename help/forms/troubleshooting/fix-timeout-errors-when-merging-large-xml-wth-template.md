---
title: Fouten in time-out verhelpen bij het samenvoegen van grote XML-gegevensbestanden met de XDP-sjabloon
description: Grote XML-bestanden samenvoegen met een sjabloon in AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
source-git-commit: 164741ce5ae7d00f904365589438c2eaaf1e05db
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Hoe te om verwezenlijking van pdf- dossiers toe te laten door grote xml- gegevensdossiers met xdp malplaatjes samen te voegen

Bij het samenvoegen van grote XML-gegevensbestanden met de xdp-sjabloon ziet u mogelijk de volgende fout in het logbestand

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Ga als volgt te werk om de bovenstaande fout op te lossen

## De time-out voor het bestand aries wijzigen

* AEM server stoppen
* Een map maken met de naam **installeren** onder de crx-quickstart-map van uw AEM-installatie
* Een bestand maken met de naam **org.apache.aries.transaction.config** met de volgende inhoud aries.transaction.timeout=&quot;1200&quot; in de installatiemap. U kunt de time-outwaarde naar wens wijzigen. De time-outwaarde is in seconden

>[!NOTE]
> Als u de configuratie org.apache.aries.transaction hebt gemaakt, kunt u de time-outwaarden voor de transactie bewerken vanuit het menu [configMgr](http://localhost:4502/system/console/configMgr) in plaats van het bestand te bewerken


## De instellingen van de Jacorb ORB-provider wijzigen

* [Open OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* Zoeken naar **Jacorb ORB Provider**
* Voeg de volgende ingang jacorb.connection.client.pending_response_timeout=600000 toe Bovenstaand plaatsen plaatst de hangende reactieonderbreking (ook genoemd geworden, cliëntonderbreking CORBA) aan 600 seconden.
* Uw wijzigingen opslaan
