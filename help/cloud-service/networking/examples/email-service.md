---
title: E-mailservice
description: Leer hoe u AEM as a Cloud Service kunt configureren om verbinding te maken met een e-mailservice met behulp van egress-poorten.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# E-mailservice

E-mails verzenden vanuit AEM as a Cloud Service door AEM te configureren `DefaultMailService` om geavanceerde voorzien van een netwerkuitgang havens te gebruiken.

Omdat (de meeste) de postdiensten niet over HTTP/HTTPS lopen, moeten de verbindingen aan de postdiensten van AEM as a Cloud Service proxied uit zijn.

+ `smtp.host` wordt ingesteld op de OSGi-omgevingsvariabele `$[env:AEM_PROXY_HOST]` het wordt dus door het egress geleid .
+ `smtp.port` is ingesteld op `portForward.portOrig` poort die is toegewezen aan de host en poort van de e-mailservice van de bestemming. In dit voorbeeld wordt de toewijzing gebruikt: `AEM_PROXY_HOST:30002` → `smtp.sendgrid.com:465`.

Aangezien geheimen niet in code moeten worden opgeslagen, kunnen de gebruikersnaam en het wachtwoord van de e-mailservice het beste worden opgegeven met [geheime OSGi configuratievariabelen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), instellen met behulp van AIO CLI of de Cloud Manager-API.

Doorgaans [flexibel poortbereik](../flexible-port-egress.md) wordt gebruikt om te voldoen aan integratie met een e-mailservice, tenzij het nodig is `allowlist` het IP van de Adobe, in welk geval [toegewezen IP-adres egress](../dedicated-egress-ip-address.md) kan worden gebruikt.

## Geavanceerde netwerkondersteuning

Het volgende codevoorbeeld wordt gesteund door de volgende geavanceerde voorzien van een netwerkopties.

| Geen geavanceerde netwerken | [Flexibele poortuitgang](../flexible-port-egress.md) | [IP-adres van specifiek egress](../dedicated-egress-ip-address.md) | [Virtueel privé netwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi-configuratie

Dit OSGi configuratievoorbeeld vormt AEM de Dienst van Mail OSGi om een externe postdienst, als volgende Manager van de Wolk te gebruiken `portForwards` de [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) bewerking.

```json
...
"portForwards": [{
    "name": "smtp.sendgrid.com",
    "portDest": 465,
    "portOrig": 30002
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST]",
    "smtp.port": "30002",
    "smtp.user": "$[env:EMAIL_USERNAME;default=apikey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

Het volgende `aio CLI` bevel kan worden gebruikt om de geheimen OSGi op een per milieubasis te plaatsen:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "apikey" --secret EMAIL_PASSWORD "password123"
```
