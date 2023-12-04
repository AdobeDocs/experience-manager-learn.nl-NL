---
title: Integreren met [!DNL ServiceNow]
description: Maak en geef alle incidenten weer met behulp van het formuliergegevensmodel.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
duration: 74
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# AEM Forms integreren met [!DNL ServiceNow]

Gebeurtenis maken en weergeven in [!DNL ServiceNow] Formuliergegevensmodel gebruiken in AEM Forms.

## Vereisten

* [!DNL ServiceNow] account.
* Vertrouwd met [gegevensbronnen maken](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Vertrouwd met [Formuliergegevensmodel](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Voorbeeldelementen

De voorbeeldelementen die bij dit artikel worden geleverd, zijn onder andere:

* Configuratie van cloudservice
* Taggebestanden om een incident te maken en alle incidenten op te halen
* Formuliergegevensmodel gebaseerd op de wagerbestanden
* Adaptief formulier voor maken en weergeven [!DNL ServiceNow] incidenten

## De elementen op uw server implementeren

* Download de [voorbeeldelementen](assets/service-now.zip)
* Elementen importeren in AEM met [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Het voor deze integratie gebruikte wagerbestand bevindt zich onder de ```/conf/9957/settings/cloudconfigs/fdm``` map in crx-opslagplaats
* Bewerk de [Configuratie van de cloudservice van CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)om aan uw instantie te passen ServiceNow.
* Bewerk de [Configuratie van de GetAllIncidents-cloudservice](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) om aan uw instantie te passen ServiceNow. U zult de gastheer, de gebruikersbenaming en het wachtwoord moeten veranderen om uw ServiceNow instantiegeloofsbrieven aan te passen.

## Toegangsreferenties van ServiceNow-instantie

* Klik op uw gebruikersprofiel
  ![klikken op gebruikersprofiel](assets/snow-1.png)

* Klik op Instantiewachtwoord beheren
* De details van de instantie worden hieronder weergegeven
  ![instantiedetails](assets/snow-3.png)

## Integratie testen

* [Het adaptieve formulier openen](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Voer enkele waarden in het veld Beschrijving en opmerkingen in en klik op de knop Incident maken
* De incident-id van het nieuwe incident moet in het tekstveld worden ingevuld en in de onderstaande tabel moeten alle incidenten worden vermeld.
