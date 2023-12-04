---
title: De voorbeeldelementen implementeren
description: Implementeer de voorbeeldbestanden op uw lokale systeem voor cloud.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
duration: 57
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# De voorbeeldelementen implementeren

Implementeer de volgende middelen op uw lokale cloudsysteem om ervoor te zorgen dat deze gebruiksaanwijzing op uw systeem werkt.

* Controleer of u alle vereiste configuraties/accounts hebt gemaakt die in het dialoogvenster[inleidende document](./introduction.md)

* [Het aangepaste adaptieve formuliersjabloon en de bijbehorende pagina-component installeren](./assets/azure-portal-template-page-component.zip)

* [Het SendGrid-formuliergegevensmodel installeren](./assets/send-grid-form-data-model.zip). Dit formuliergegevensmodel werkt alleen als u uw API-sleutel opgeeft en SendGrid van het adres is geverifieerd. Het formuliergegevensmodel testen in de formuliergegevensmodeleditor

* [Het Azure-ondersteunde formuliergegevensmodel installeren](./assets/azure-storage-fdm.zip). Het formuliergegevensmodel werkt alleen als u uw aanmeldingsgegevens voor uw Azure Storage-account opgeeft. Test het gegevensmodel van het formulier in de formuliergegevensmodeleditor.

* [Het adaptieve voorbeeldformulier importeren](./assets/credit-applications-af.zip)
* [De clientbibliotheek importeren](./assets/client-lib.zip)
* [Een voorbeeld van het formulier bekijken](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). Voer een geldige e-mail in en klik op Opslaan. De formuliergegevens moeten worden opgeslagen in de Azure Storage en er wordt een e-mail met een koppeling naar het opgeslagen formulier verzonden naar het opgegeven e-mailadres.
