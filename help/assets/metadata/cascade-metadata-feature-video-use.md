---
title: Cascading Metadata in AEM Assets gebruiken
seo-title: Cascading Metadata in AEM Assets gebruiken
description: Met geavanceerd metagegevensbeheer kunnen gebruikers trapsgewijze veldregels maken om contextuele relaties tussen metagegevens in AEM Assets te vormen. In de onderstaande video ziet u nieuwe dynamische regels voor veldvereisten, zichtbaarheid en contextuele keuzes. In de video worden ook de stappen beschreven die een beheerder nodig heeft om deze regels toe te passen op een aangepast metagegevensschema.
seo-description: Met geavanceerd metagegevensbeheer kunnen gebruikers trapsgewijze veldregels maken om contextuele relaties tussen metagegevens in AEM Assets te vormen. In de onderstaande video ziet u nieuwe dynamische regels voor veldvereisten, zichtbaarheid en contextuele keuzes. In de video worden ook de stappen beschreven die een beheerder nodig heeft om deze regels toe te passen op een aangepast metagegevensschema.
uuid: 470c1b1a-f888-4c90-87d7-acfa9a5fa6b1
discoiquuid: ccd1acb1-bb7f-48c2-91e0-cccbeedad831
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---


# Cascading Metadata in AEM Assets{#using-cascading-metadata-in-aem-assets} gebruiken

Met geavanceerd metagegevensbeheer kunnen gebruikers trapsgewijze veldregels maken om contextuele relaties tussen metagegevens in AEM Assets te vormen. In de onderstaande video ziet u nieuwe dynamische regels voor veldvereisten, zichtbaarheid en contextuele keuzes. In de video worden ook de stappen beschreven die een beheerder nodig heeft om deze regels toe te passen op een aangepast metagegevensschema.

>[!VIDEO](https://video.tv.adobe.com/v/20702/?quality=9&learn=on)

Er zijn drie dynamische regelsets die voor een bepaald metagegevensveld kunnen worden ingeschakeld:

1. **Voorschrift** : een veld kan dynamisch worden gemarkeerd als vereist om te worden gebaseerd op de waarde van een ander vervolgkeuzeveld.

2. **Zichtbaarheid** : velden kunnen altijd zichtbaar zijn of alleen zichtbaar zijn op basis van de waarde van een ander vervolgkeuzeveld.

3. **Keuzen** : (alleen van toepassing op vervolgkeuzelijsten) filtert u de keuzen die aan de gebruiker worden getoond op basis van de momenteel geselecteerde waarde van een ander vervolgkeuzeveld.

>[!NOTE]
>
>Cascading rules can only be created based on the value(s) of a dropdown field. Het is mogelijk om alle drie regelreeksen op het zelfde meta-gegevensgebied toe te passen maar als beste praktijk wordt het geadviseerd om elke regelreeks afhankelijk van de zelfde meta-gegevensdrop-down te maken.

[Aangepast metagegevenspakket](assets/cascade-metadata-values-001.zip) downloaden

## Aanvullende bronnen{#additional-resources}

Aangepast metagegevensschema gemaakt op: `/conf/global/settings/dam/adminui-extension/metadataschema/custom`. Met het onderstaande AEM-pakket wordt een aangepast schema op de map toegepast: `/content/dam/we-retail/en/activities`:

