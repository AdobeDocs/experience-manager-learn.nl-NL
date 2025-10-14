---
title: Integreren met  [!DNL ServiceNow]
description: Maak en geef alle incidenten weer met behulp van het formuliergegevensmodel.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
duration: 47
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# AEM Forms integreren met [!DNL ServiceNow]

Gebeurtenissen maken en weergeven in [!DNL ServiceNow] met gebruik van het formuliergegevensmodel in AEM Forms.

## Vereisten

* [!DNL ServiceNow] account.
* Vertrouwd met [&#x200B; het creëren van gegevensbronnen &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=nl-NL)
* Vertrouwd met [&#x200B; Model van de Gegevens van de Vorm &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=nl-NL)

## Voorbeeld-Assets

De voorbeeldelementen die bij dit artikel worden geleverd, zijn onder andere:

* Configuratie van cloudservice
* Waggerbestanden om een incident te maken en alles op te halen   incidenten
* Formuliergegevensmodel gebaseerd op de wagerbestanden
* Aangepast formulier om [!DNL ServiceNow] -incidenten te maken en weer te geven

## De elementen op uw server implementeren

* Download de [&#x200B; steekproefactiva &#x200B;](assets/service-now.zip)
* Importeer de activa in AEM gebruikend [&#x200B; pakketmanager &#x200B;](http://localhost:4502/crx/packmgr/index.jsp)
* Het kwikbestand dat voor deze integratie wordt gebruikt, bevindt zich onder de map ```/conf/9957/settings/cloudconfigs/fdm``` in de crx-opslagplaats
* Bewerk de [&#x200B; CreateIncident de dienstconfiguratie van de wolk &#x200B;](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident) om uw instantie aan te passen ServiceNow.
* Bewerk de [&#x200B; GetAllIncidents de dienstconfiguratie van de wolk &#x200B;](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) om uw instantie aan te passen ServiceNow. U zult de gastheer, de gebruikersbenaming en het wachtwoord moeten veranderen om uw ServiceNow instantiegeloofsbrieven aan te passen.

## Toegangsreferenties van ServiceNow-instantie

* Klik op uw gebruikersprofiel
  ![&#x200B; klik op gebruikersprofiel &#x200B;](assets/snow-1.png)

* Klik op Instantiewachtwoord beheren
* De details van de instantie worden hieronder weergegeven
  ![&#x200B; instantiedetails &#x200B;](assets/snow-3.png)

## Integratie testen

* [&#x200B; open de Aangepaste Vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Voer enkele waarden in het veld Beschrijving en opmerkingen in en klik op de knop Incident maken
* De incident-id van het nieuwe incident moet in het tekstveld worden ingevuld en in de onderstaande tabel moeten alle incidenten worden vermeld.
