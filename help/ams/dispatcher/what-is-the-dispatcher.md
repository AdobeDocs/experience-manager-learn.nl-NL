---
title: Wat is "De verzender"?
description: Begrijp wat een Dispatcher eigenlijk is.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 829ad9733b4326c79b9b574b13b1d4c691abf877
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 1%

---


# Wat is &quot;De verzender&quot;?

[Inhoudsopgave](./overview.md)

Begin met de basisbeschrijving van wat een AEM Dispatcher impliceert.

## Apache Web Server

Begin met een basisinstallatie van Apache Web Server op een Linux-server.

Basisuitleg van wat een Apache-server doet:

- Volg eenvoudige regels om bestanden via de HTTP(S)-protocollen vanuit de statische documentmap te bedienen (`DocumentRoot`)
- Bestanden die op een standaardlocatie zijn opgeslagen (`/var/www/html`) op aanvragen worden afgehandeld en in de browser van de verzoekende client worden weergegeven




## AEM bestand met specifieke modules (`mod_dispatcher.so`)

Voeg vervolgens een plug-in toe aan de Apache Web Server, de zogenaamde Dispatcher-module

Basisuitleg van wat de module Adobe AEM Dispatcher doet:

- Hiermee wordt de standaardbestandshandler vergroot
- Filtert uit slechte verzoeken/beschermt AEM zachte buik/eindpunten
- Taakbalansen als er meer dan één renderer aanwezig is
- Staat voor een levende geheim voorgeheugenfolder/steunt het spoelen van stagnerende dossiers toe
- Het is de voordeur voor alle installaties van AMS en het levert websites en activa aan browser van de cliënt
- Het plaatst verzoeken om tegen een veel sneller tarief opnieuw te dienen dan een AEM server op zijn kon uitvoeren
- Veel meer...

## Workflow voor webverkeer

Als u begrijpt welke onderdelen samen zijn geïnstalleerd om een standaard Dispatcher-server te maken, krijgt u begrip van de basisworkflow voor webverkeer voor een Adobe Manager-configuratie.
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