---
title: Transformaties met AEM Forms
description: Deel 2 van de integratie van Acrobat met AEM Forms. Maak een schema van een Acrobat-formulier.
feature: adaptive-forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 34
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 0%

---


# Schema maken van het acroform

De volgende stap bestaat uit het maken van een schema op basis van het Acrobat-formulier dat u in de vorige stap hebt gemaakt. Er is een voorbeeldtoepassing beschikbaar om het schema te maken als onderdeel van deze zelfstudie. Volg de volgende instructies om het schema te maken:

1. Login aan [&#x200B; CRXDE Lite &#x200B;](http://localhost:4502/crx/de)
2. Openen naar het bestand `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Wijzig `saveLocation` in een geschikte map op de vaste schijf. Zorg ervoor dat de map waarin u opslaat, al is gemaakt.
4. Wijs uw browser aan [&#x200B; XSD &#x200B;](http://localhost:4502/content/DocumentServices/CreateXsd.html) pagina tot stand brengen die op AEM wordt ontvangen.
5. Sleep de Acroform.
6. Controleer de map die in Stap 3 is opgegeven. Het schemabestand wordt opgeslagen op deze locatie.

## Acroform uploaden

Deze demo werkt alleen op uw systeem als u een map maakt met de naam `acroforms` in AEM Assets. Upload het formulier naar deze `acroforms` -map.

>[!NOTE]
>
>De voorbeeldcode zoekt naar het acroform in deze map. Het acroform is nodig om de verzonden gegevens van het adaptieve formulier samen te voegen.
