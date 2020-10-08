---
title: Transformaties met AEM Forms
seo-title: Adaptieve formuliergegevens samenvoegen met Acrobat
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---


# Schema maken van het acroform

De volgende stap bestaat uit het maken van een schema op basis van het Acrobat-formulier dat u in de vorige stap hebt gemaakt. Er is een voorbeeldtoepassing beschikbaar om het schema te maken als onderdeel van deze zelfstudie. Volg de volgende instructies om het schema te maken:

1. Aanmelden bij [CRXDE Lite](http://localhost:4502/crx/de)
2. Openen naar het bestand `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Wijzig de map `saveLocation` in een geschikte map op de vaste schijf. Zorg ervoor dat de map waarin u opslaat, al is gemaakt.
4. Wijs in uw browser naar de XSD [-pagina](http://localhost:4502/content/DocumentServices/CreateXsd.html) maken die wordt gehost op AEM.
5. Sleep de Acroform.
6. Controleer de map die in Stap 3 is opgegeven. Het schemabestand wordt opgeslagen op deze locatie.

## Acroform uploaden

Deze demo werkt alleen op uw systeem als u een map maakt met de naam `acroforms` AEM Assets. Upload het formulier naar deze `acroforms` map.

>[!NOTE]
>
>De voorbeeldcode zoekt naar het acroform in deze map. Het acroform is nodig om de verzonden gegevens van het adaptieve formulier samen te voegen.
