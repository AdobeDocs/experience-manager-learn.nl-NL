---
title: Tabelcomponent gebruiken in AEM Forms Print Channel-document
seo-title: Tabelcomponent gebruiken in AEM Forms Print Channel-document
description: In de volgende video worden de stappen doorlopen die zijn vereist om tabelcomponent te gebruiken in Interactieve communicatie voor documenten met afdrukkanalen.
feature: Interactive Communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---


# Tabelcomponent gebruiken in AEM Forms Print Channel Document {#using-table-component-in-aem-forms-print-channel-document}

In de volgende video worden de stappen doorlopen die zijn vereist om tabelcomponent te gebruiken in Interactieve communicatie voor documenten met afdrukkanalen.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

Tabellen worden gebruikt om gegevens in tabelvorm weer te geven. De rijen in de lijst moeten afhankelijk van de gegevens groeien of krimpen die door de gegevensbron zijn teruggekeerd. Als u een tabel wilt gebruiken in een document met afdrukkanalen, moet u een lay-outbestand (xdp-bestand) maken met AEM Forms Designer. In dit lay-outdossier, voegen wij de lijst met het vereiste aantal kolommen toe. Zorg ervoor dat het objecttype van het kolomveld, afhankelijk van uw vereisten, TextField of Numeriek veld is. Voor elke kolom zorgen de velden ervoor dat de gegevensbinding is ingesteld op Naam gebruiken.

>[!NOTE]
>
>Als u een tabel dynamisch wilt maken, moet u de rij als herhalend hebben gemarkeerd.

**Probeer het op uw eigen server**

* [Download en decomprimeer het bestand assets op de vaste schijf](assets/usingtablesinprintchannel.zip)

* De twee ZIP-bestanden importeren in AEM met pakketbeheer

* De volgende elementen zijn opgenomen in de elementen die aan dit artikel zijn gekoppeld:

   * Lay-outfragment

   * Formuliergegevensmodel

   * Interactief communicatiedocument
   * sampleretirementaccountdata.json

* Open het Interactieve Communicatie Document op [geef wijze ](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html) uit.

* Voeg het de lay-outfragment TableDemo aan de bijdragesectie toe.
* De tabelcellen binden aan de juiste formuliergegevensmodelelementen, zoals in de video wordt getoond

* Geef een voorvertoning weer van het interactieve communicatiedocument met het voorbeeldgegevensbestand voor de JPEG-gegevens dat u ontvangt

