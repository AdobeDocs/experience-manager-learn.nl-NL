---
title: Dynamische media voor transparantie en batchverwerking van inhoudautomatisering
description: Leer hoe u Dynamic Media in AEM gebruikt om virtuele uitvoeringen te maken, transparantie te beheren en beeldverwerking te automatiseren voor schaalbaar hergebruik van inhoud.
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 560
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
source-git-commit: a509b7c41e6adb05e6c3b791ac2e99f635973322
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---


# Dynamische media voor transparantie en batchverwerking van inhoudautomatisering

Leer hoe u Dynamic Media in AEM gebruikt om virtuele uitvoeringen te maken, transparantie te beheren en beeldverwerking te automatiseren voor schaalbaar hergebruik van inhoud.

>[!VIDEO](https://video.tv.adobe.com/v/3463051/?learn=on&enablevpops&captions=dut)


## Voorbeeld van dynamische media-elementen

Hieronder ziet u voorbeelden van Dynamic Media-elementen en de bijbehorende URL&#39;s die in de video worden gebruikt.

>[!BEGINTABS]

>[!TAB  de transparantievoorbeelden van het Beeld ]

Hieronder ziet u de voorbeeld-URL&#39;s van de Dynamic Media Image Server die in de video worden gebruikt.

| Voorvertoning | Beschrijving | URL van dynamische media |
|-----------|------------------|---------|
| ![ Gebrek ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | Standaard | [ Verbinding ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![ Samengesteld met naadloze laag van het achtergrondbeeld ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Samengesteld met naadloze achtergrondafbeeldingslaag | [ Verbinding ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![ Rode Achtergrond ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Rode achtergrond | [ Verbinding ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![ Uitgeknipt aan ovaal weg ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255){width="250"} | Uitgeknipt naar ovaal pad | [ Verbinding ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255) |


>[!TAB  de wegvoorbeelden van het Beeld ]

Hieronder ziet u de voorbeeld-URL&#39;s van de Dynamic Media Image Server die in de video worden gebruikt.

| Voorvertoning | Beschrijving | URL van dynamische media |
|-----------|------------------|---------|
| ![ genormaliseerd aan 80 pixel breed (geen transparantie) ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | Genormaliseerd aan 80 pixel breed (geen transparantie) {width="250"} | [ Verbinding ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![ Gewas aan Weg ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800){width="250"} | Uitsnijden naar pad | [ Verbinding ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800) |
| ![ Clip aan Weg ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800){width="250"} | Bijsnijden naar pad | [ Verbinding ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800) |
| ![ Clip aan Weg en Uitsnijden aan weg ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800){width="250"} | Bijsnijden naar pad en uitsnijden naar pad | [ Verbinding ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800) |
| ![ Clip aan een andere weg ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800){width="250"} | Bijsnijden naar een ander pad | [ Verbinding ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800) |
| ![ Knip aan een andere weg en maak rode achtergrond ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800){width="250"} | Naar een ander pad knippen en een rode achtergrond maken | [ Verbinding ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800) |

>[!ENDTABS]


## Dynamic Media Image Server-API&#39;s

* [ Dynamisch Beeld van Media die en API teruggeven dienen ](https://experienceleague.adobe.com/nl/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [ Dynamische voorproef van de Momentopname van Media ](https://snapshot.scene7.com/)
