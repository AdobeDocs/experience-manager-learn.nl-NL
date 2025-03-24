---
title: Fouten in time-out verhelpen bij het samenvoegen van grote XML-gegevensbestanden met de XDP-sjabloon
description: Grote XML-bestanden samenvoegen met een sjabloon in AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: Experience Manager 6.5
feature: Output Service,Forms Service
topic: Administration
jira: KT-11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
duration: 37
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---

# Het maken van pdf-bestanden inschakelen door grote XML-gegevensbestanden samen te voegen met xdp-sjablonen

Bij het samenvoegen van grote XML-gegevensbestanden met de xdp-sjabloon ziet u mogelijk de volgende fout in het logbestand

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Ga als volgt te werk om de bovenstaande fout op te lossen

## De time-out voor het bestand aries wijzigen

* AEM-server stoppen
* Creeer een omslag genoemd **installeer** onder de crx-quickstart omslag van uw installatie van AEM
* Creeer een dossier genoemd **org.apache.aries.transaction.config** met de volgende inhoud
aries.transaction.timeout=&quot;1200&quot;
onder installatiemap. U kunt de time-outwaarde naar wens wijzigen. De time-outwaarde is in seconden

>[!NOTE]
> Zodra u de configuratie org.apache.aries.transaction creeert kunt u de waarden van de transactieduur uit [ configMgr ](http://localhost:4502/system/console/configMgr) in plaats van het uitgeven van het dossier uitgeven


## De instellingen van de Jacorb ORB-provider wijzigen

* [ Open OSGi ConfigMgr ](http://localhost:4502/system/console/configMgr)
* Onderzoek naar **Jacorb ORB Provider**
* De volgende vermelding toevoegen
jacorb.connection.client.pending_response_timeout=600000
De bovenstaande instelling stelt de wachttijd voor de antwoordtime-out (ook wel CORBA-client-timeout genoemd) in op 600 seconden.
* Uw wijzigingen opslaan
