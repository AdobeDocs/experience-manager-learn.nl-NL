---
title: De oplossing testen
description: Voer Main.java uit om de oplossing te testen
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Adaptieve Forms
topic: Ontwikkeling
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---


# Eclipproject importeren

[zip file](./assets/aem-forms-doc-gen.zip) downloaden

Eclipse starten en het project importeren in Eclipse
Het project bevat de volgende bestanden in de bronnenmap:

* DataFile1 en DataFile2 - Voorbeeld van XML-gegevensbestanden die met de sjabloon moeten worden samengevoegd om het definitieve PDF-bestand te genereren
* address.xdp - XDP-sjabloon
* service_token.json - U moet de inhoud van dit bestand vervangen door uw accountspecifieke gegevens
* options.json - De opties die in dit bestand worden opgegeven, worden gebruikt om de eigenschappen in te stellen van het PDF-bestand dat wordt gegenereerd door de API

![resources-bestand](./assets/resource-files.JPG)

## De oplossing testen

* Kopieer en plak uw de dienstgeloofsbrieven in het dienst_token.json middeldossier in het project.
* Open het bestand DocumentGeneration.java en geef de map op waarin u de gegenereerde PDF-bestanden wilt opslaan
* Open Main.java. Stel de waarde van de variabele postURL zo in dat deze naar de instantie wijst.
* De toepassing Main.java uitvoeren als Java

>[!NOTE]
> De allereerste keer dat u het Java-programma uitvoert, wordt een HTTP 403-fout gegenereerd. Om voorbij dit te komen, zorg ervoor u [aangewezen toestemmingen aan de technische rekeningsgebruiker in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem) geeft.

**AEM Forms** gebruikt de rol die ik voor deze cursus heb gebruikt.

