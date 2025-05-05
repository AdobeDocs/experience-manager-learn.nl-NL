---
title: De oplossing testen
description: Voer Main.java uit om de oplossing te testen
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
duration: 43
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Eclipproject importeren

Download en unzip het [ zip dossier ](./assets/aem-forms-cs-doc-gen.zip)

Eclipse starten en het project importeren in Eclipse
Het project bevat de volgende bestanden in de bronnenmap:

* DataFile1, DataFile2 en DataFile3 - Voorbeeld van XML-gegevensbestanden die met de sjabloon moeten worden samengevoegd om het uiteindelijke PDF-bestand te genereren
* custom_fonts.xdp - XDP-sjabloon.
* service_token.json - U moet de inhoud van dit bestand vervangen door uw accountspecifieke gegevens
* options.json - De opties die in dit bestand worden opgegeven, worden gebruikt om de eigenschappen in te stellen van het PDF-bestand dat wordt gegenereerd door de API

![ middelen-dossier ](./assets/resource-files.png)

## De oplossing testen

* Kopieer en plak uw de dienstgeloofsbrieven in het dienst_token.json middeldossier in het project.
* Open het bestand DocumentGeneration.java en geef de map op waarin u de gegenereerde PDF-bestanden wilt opslaan
* Open Main.java. Stel de waarde van de variabele postURL zo in dat deze naar de instantie wijst.
* De toepassing Main.java uitvoeren als Java

>[!NOTE]
> De allereerste keer dat u het Java-programma uitvoert, wordt een HTTP 403-fout gegenereerd. Om voorbij dit te krijgen zorg u de [ aangewezen toestemmingen aan de technische rekeningsgebruiker in AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=nl-NL#configure-access-in-aem) geeft.

**de Gebruikers van AEM Forms** is de rol ik voor deze cursus heb gebruikt.
