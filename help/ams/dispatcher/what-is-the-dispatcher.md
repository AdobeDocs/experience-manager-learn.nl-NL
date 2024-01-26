---
title: Wat is "De verzender"?
description: Begrijp wat een Dispatcher eigenlijk is.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 80
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Wat is &quot;De verzender&quot;?

[Inhoudsopgave](./overview.md)

Begin met de basisbeschrijving van wat een AEM Dispatcher impliceert.

## Apache Web Server

Begin met een basisinstallatie van Apache Web Server op een Linux-server.

Basisuitleg van wat een Apache-server doet:

- Volg eenvoudige regels om bestanden via de HTTP(S)-protocollen vanuit de statische documentmap (`DocumentRoot`)
- Bestanden die op een standaardlocatie zijn opgeslagen (`/var/www/html`) op aanvragen worden afgehandeld en in de browser van de verzoekende client worden weergegeven




## AEM bestand met specifieke modules (`mod_dispatcher.so`)

Voeg vervolgens een plug-in toe aan de Apache Web Server, de zogenaamde Dispatcher-module

Basisuitleg van wat de module Adobe AEM Dispatcher doet:

- Hiermee wordt de standaardbestandshandler versterkt
- Filtert uit slechte verzoeken/beschermt AEM zachte buik/eindpunten
- Taakbalansen als er meer dan één renderer aanwezig is
- Staat voor een levende geheim voorgeheugenfolder/steunt het spoelen van stagnerende dossiers toe
- Het is de voordeur voor alle installaties van AMS en het levert websites en activa aan browser van de cliënt
- Het plaatst verzoeken om tegen een veel sneller tarief opnieuw te dienen dan een AEM server op zijn kon uitvoeren
- Veel meer...

## Workflow voor webverkeer

Begrijpen welke stukken samen worden geïnstalleerd om een basisserver van de Verzender te bouwen leidt ons om u te hebben de basiswerkschema van het Webverkeer voor een configuratie van de Diensten van de Manager van de Adobe begrijpen.
Dit zou u moeten helpen begrijpen welke rol het in de ketting van systemen speelt die inhoud aan bezoekers van uw AEM inhoud dienen.

<b>Reeds in cache opgeslagen inhoud verzenden</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>Vernieuwde inhoud uit AEM verzenden</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if NOT found 
                → requests content from publisher 
                    → publisher sends content 
                        → Dispatcher adds content to cache and replies 
                            → End User
```

<b>Publiceren/wijzigen van inhoud</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[Volgende -> Basisbestandsindeling](./basic-file-layout.md)
