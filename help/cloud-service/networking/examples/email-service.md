---
title: E-mailservice
description: Leer hoe u AEM as a Cloud Service configureert voor verbinding met een e-mailservice met behulp van egress-poorten.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 76
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# E-mailservice

Verzend e-mailberichten vanuit AEM as a Cloud Service door AEM `DefaultMailService` te configureren voor het gebruik van geavanceerde poorten voor netwerktoegang.

Omdat (de meeste) de postdiensten niet over HTTP/HTTPS lopen, moeten de verbindingen aan de postdiensten van AEM as a Cloud Service proxied uit zijn.

+ `smtp.host` wordt ingesteld op de OSGi-omgevingsvariabele `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` , zodat deze door het egress wordt geleid.
   + `$[env:AEM_PROXY_HOST]` is een gereserveerde variabele die AEM as a Cloud Service toewijst aan de interne `proxy.tunnel` -host.
   + Probeer NIET om `AEM_PROXY_HOST` via Cloud Manager in te stellen.
+ `smtp.port` wordt ingesteld op de `portForward.portOrig` -poort die wordt toegewezen aan de host en poort van de e-mailservice van het doel. In dit voorbeeld wordt de toewijzing `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465` gebruikt.
   + `smpt.port` wordt geplaatst aan de `portForward.portOrig` haven, en NIET de daadwerkelijke haven van de server SMTP. De toewijzing tussen de `smtp.port` en de `portForward.portOrig` -poort wordt bepaald door de Cloud Manager `portForwards` -regel (zoals hieronder wordt getoond).

Aangezien de geheimen niet in code moeten worden opgeslagen, worden de gebruikersbenaming en het wachtwoord van de e-maildienst best verstrekt gebruikend [ geheime OSGi configuratievariabelen ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), geplaatst gebruikend AIO CLI, of Cloud Manager API.

Typisch, [ flexibel havenuitgang ](../flexible-port-egress.md) wordt gebruikt om het integreren met de e-maildienst tevreden te stellen tenzij het aan `allowlist` Adobe IP noodzakelijk is, waarin [ specifiek adres van de uitgang ](../dedicated-egress-ip-address.md) kan worden gebruikt.

Bovendien, herzie de documentatie van AEM op [ verzendend e-mail ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email).

## Geavanceerde netwerkondersteuning

Het volgende codevoorbeeld wordt gesteund door de volgende geavanceerde voorzien van een netwerkopties.

Verzeker [ aangewezen ](../advanced-networking.md#advanced-networking) geavanceerde voorzien van een netwerkconfiguratie voorafgaand aan het volgen van dit leerprogramma is opstelling.

| Geen geavanceerde netwerken | [ Flexibele havenuitgang ](../flexible-port-egress.md) | [ Dedicated egress IP adres ](../dedicated-egress-ip-address.md) | [ Virtueel Privé Netwerk ](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi-configuratie

Dit OSGi- configuratievoorbeeld vormt de Dienst van de Post OSGi van AEM om een externe postdienst, als volgende Cloud Manager `portForwards` regel van de [ enableEnvironmentAdvancedNetworkingConfiguration ](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) verrichting te gebruiken.

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30465
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

Vorm AEM [ DefaultMailService ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) zoals vereist door uw e-mailleverancier (b.v. `smtp.ssl`, enz.).

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30465",
    "smtp.user": "$[env:EMAIL_USERNAME;default=myApiKey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

De variabele `EMAIL_USERNAME` en `EMAIL_PASSWORD` OSGi en het geheim kunnen per milieu worden geplaatst, gebruikend één van beiden:

+ [ de Configuratie van het Milieu van Cloud Manager ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ of met de opdracht `aio CLI`

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
