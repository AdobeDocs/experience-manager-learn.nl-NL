---
title: Integratie met nu service
description: Maak en geef alle incidenten weer met behulp van het formuliergegevensmodel.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 0%

---

# AEM Forms integreren met servicenow

Creeer en toon incident in ServiceNow gebruikend het Model van de Gegevens van het Vorm in AEM Forms.

## Vereisten

* ServiceNow-account.
* Vertrouwd met [gegevensbronnen maken](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Vertrouwd met [Formuliergegevensmodel](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Voorbeeldelementen

De voorbeeldelementen die bij dit artikel worden geleverd, zijn onder andere:
* Configuratie van cloudservice
* Taggebestanden om een incident te maken en alle incidenten op te halen
* Formuliergegevensmodel gebaseerd op de wagerbestanden
* Adaptief formulier voor het maken en weergeven van servicenow-incidenten

## De elementen op uw server implementeren

* Download de [voorbeeldelementen](assets/service-now.zip)
* Elementen importeren in AEM met [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Bewerk de [Configuratie van de cloudservice van CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)om aan uw instantie te passen ServiceNow.
* Bewerk de [Configuratie van de GetAllIncidents-cloudservice](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) om uw instantie ServiceNow aan te passen


## Integratie testen

* [Het adaptieve formulier openen](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Voer enkele waarden in het veld Beschrijving en opmerkingen in en klik op de knop Incident maken
* De incident-id van het nieuwe incident moet in het tekstveld worden ingevuld en in de onderstaande tabel moeten alle incidenten worden vermeld.

