---
title: Transformaties met AEM Forms
description: Deel 2 van de integratie van Acrobat met AEM Forms. Maak een schema van een Acrobat-formulier.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 34
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 0%

---


# Schema maken van het acroform

De volgende stap bestaat uit het maken van een schema op basis van het Acrobat-formulier dat u in de vorige stap hebt gemaakt. Er is een voorbeeldtoepassing beschikbaar om het schema te maken als onderdeel van deze zelfstudie. Volg de volgende instructies om het schema te maken:

1. Aanmelden bij [CRXDE Lite](http://localhost:4502/crx/de)
2. Openen naar het bestand `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Wijzig de `saveLocation` naar een geschikte map op uw vaste schijf. Zorg ervoor dat de map waarin u opslaat, al is gemaakt.
4. Wijs uw browser aan [XSD maken](http://localhost:4502/content/DocumentServices/CreateXsd.html) op AEM.
5. Sleep de Acroform.
6. Controleer de map die in Stap 3 is opgegeven. Het schemabestand wordt opgeslagen op deze locatie.

## Acroform uploaden

Deze demo werkt alleen op uw systeem als u een map maakt met de naam `acroforms` in AEM Assets. Acroform uploaden naar dit `acroforms` map.

>[!NOTE]
>
>De voorbeeldcode zoekt naar het acroform in deze map. Het acroform is nodig om de verzonden gegevens van het adaptieve formulier samen te voegen.
