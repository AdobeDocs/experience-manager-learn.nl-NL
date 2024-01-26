---
title: Tabelcomponent gebruiken in AEM Forms Print Channel-document
description: In de volgende video worden de stappen doorlopen die zijn vereist om tabelcomponent te gebruiken in Interactieve communicatie voor documenten met afdrukkanalen.
feature: Interactive Communication
doc-type: technical video
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
duration: 293
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Tabelcomponent gebruiken in AEM Forms Print Channel-document {#using-table-component-in-aem-forms-print-channel-document}

In de volgende video worden de stappen doorlopen die zijn vereist om tabelcomponent te gebruiken in Interactieve communicatie voor documenten met afdrukkanalen.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=12&learn=on)

Tabellen worden gebruikt om gegevens in tabelvorm weer te geven. De rijen in de lijst moeten afhankelijk van de gegevens groeien of krimpen die door de gegevensbron zijn teruggekeerd. Als u een tabel wilt gebruiken in een document met afdrukkanalen, moet u een lay-outbestand (xdp-bestand) maken met AEM Forms Designer. In dit lay-outdossier, voegen wij de lijst met het vereiste aantal kolommen toe. Zorg ervoor dat het objecttype van het kolomveld, afhankelijk van uw vereisten, TextField of Numeriek veld is. Voor elke kolom zorgen de velden ervoor dat de gegevensbinding is ingesteld op Naam gebruiken.

>[!NOTE]
>
>Als u een tabel dynamisch wilt maken, moet u de rij als herhalend hebben gemarkeerd.

**Probeer het op uw eigen server**

* [Download en decomprimeer het bestand assets op de vaste schijf](assets/usingtablesinprintchannel.zip)

* De twee ZIP-bestanden importeren in AEM met pakketbeheer

* De volgende elementen zijn opgenomen in de elementen die aan dit artikel zijn gekoppeld:

   * Lay-outfragment

   * Formuliergegevensmodaal

   * Interactief communicatiedocument
   * sampleretirementaccountdata.json

* Open het interactieve communicatiedocument in [bewerkingsmodus](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Voeg het de lay-outfragment TableDemo aan de bijdragesectie toe.
* De tabelcellen binden aan de juiste formuliergegevensmodelelementen, zoals in de video wordt getoond

* Geef een voorvertoning weer van het interactieve communicatiedocument met het voorbeeldgegevensbestand voor de JPEG-gegevens dat u ontvangt
