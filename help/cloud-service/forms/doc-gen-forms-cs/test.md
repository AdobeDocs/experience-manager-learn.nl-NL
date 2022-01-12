---
title: De oplossing testen
description: Voer Main.java uit om de oplossing te testen
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: f712e86600ed18aee43187a5fb105324b14b7b89
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---


# Eclipproject importeren

Download en decomprimeer de [zip-bestand](./assets/aem-forms-cs-doc-gen.zip)

Start Eclipse en importeer het project in Eclipse. Het project bevat de volgende bestanden in de bronnenmap:

* DataFile1, DataFile2 en DataFile3 - Voorbeeld van XML-gegevensbestanden die met de sjabloon moeten worden samengevoegd om het uiteindelijke PDF-bestand te genereren
* custom_fonts.xdp - XDP-sjabloon.
* service_token.json - U moet de inhoud van dit bestand vervangen door uw accountspecifieke gegevens
* options.json - De opties die in dit bestand worden opgegeven, worden gebruikt om de eigenschappen in te stellen van het PDF-bestand dat door de API wordt gegenereerd

![resources-bestand](./assets/resource-files.png)

## De oplossing testen

* Kopieer en plak uw de dienstgeloofsbrieven in het dienst_token.json middeldossier in het project.
* Open het bestand DocumentGeneration.java en geef de map op waarin u de gegenereerde PDF-bestanden wilt opslaan
* Open Main.java. Stel de waarde van de variabele postURL zo in dat deze naar de instantie wijst.
* De toepassing Main.java uitvoeren als Java

>[!NOTE]
> De allereerste keer dat u het Java-programma uitvoert, wordt een HTTP 403-fout gegenereerd. Om voorbij dit te komen, zorg ervoor u [de juiste machtigingen voor de gebruiker van de technische account in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms-gebruikers** Dat is de rol die ik voor deze cursus heb gebruikt.

