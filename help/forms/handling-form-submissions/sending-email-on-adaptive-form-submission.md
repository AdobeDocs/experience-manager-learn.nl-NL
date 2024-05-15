---
title: E-mail verzenden bij verzenden van adaptief formulier
description: E-mail met bevestiging verzenden bij het verzenden van het adaptieve formulier met het onderdeel E-mail verzenden
feature: Adaptive Forms
doc-type: article
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
duration: 40
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# E-mail verzenden bij verzenden van adaptief formulier {#sending-email-on-adaptive-form-submission}

Een van de gebruikelijke acties is het sturen van een bevestigingsbericht naar de indiener bij een geslaagde verzending van het adaptieve formulier. Hiervoor selecteren we de actie E-mail verzenden als verzendactie.

U kunt e-mailsjabloon gebruiken of alleen de tekst van het e-mailbericht typen, zoals hieronder in deze schermafbeelding wordt weergegeven.

U ziet de syntaxis voor het invoegen van waarden voor formuliervelden in de e-mail. U kunt ook formulierbijlagen in de e-mail opnemen door het selectievakje Bijlagen opnemen in de configuratie-eigenschappen in te schakelen.

Wanneer het adaptieve formulier wordt verzonden, ontvangt de ontvanger een e-mail.

![SendEmail](assets/sendemailaction.gif)

## Benodigde configuraties {#configurations-needed}

U moet de CQ-mailservice op de dag configureren. Dit kan worden gevormd door uw browser aan te wijzen [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

In de schermafbeelding ziet u de configuratie-eigenschappen voor de Adobe-mailserver.

![mailservice](assets/mailservice.png)

Volg de onderstaande instructies om dit op uw server te proberen:

* [Elementen importeren](assets/timeoffrequest.zip) gekoppeld aan dit artikel in AEM met de pakketmanager.

* Open de [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Vul de gegevens in.Geef een geldig e-mailadres op in het veld E-mail.

* Verzend het formulier.
