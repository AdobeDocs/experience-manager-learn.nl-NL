---
title: Wat is "De Dispatcher"
description: Begrijp wat een Dispatcher eigenlijk is.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 73
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Wat is &quot;De Dispatcher&quot;

[Inhoudsopgave](./overview.md)

Om te beginnen de basisbeschrijving van wat een AEM Dispatcher inhoudt.

## Apache Web Server

Begin met een basisinstallatie van Apache Web Server op een Linux-server.

Basisuitleg van wat een Apache-server doet:

- Volg eenvoudige regels om dossiers over de HTTP(s) protocollen van zijn statische documentfolder (`DocumentRoot`) te dienen
- Bestanden die op een standaardlocatie zijn opgeslagen (`/var/www/html`), worden op aanvragen afgehandeld en in de browser van de desbetreffende client weergegeven




## AEM bestand met specifieke modules (`mod_dispatcher.so`)

Voeg vervolgens een plug-in toe aan de Apache Web Server, de zogenaamde Dispatcher-module

Basisuitleg van wat de Adobe AEM Dispatcher module doet:

- Hiermee wordt de standaardbestandshandler versterkt
- Filtert uit slechte verzoeken/beschermt AEM zachte buik/eindpunten
- Taakbalansen als er meer dan één renderer aanwezig is
- Staat voor een levende geheim voorgeheugenfolder/steunt het spoelen van stagnerende dossiers toe
- Het is de voordeur voor alle installaties van AMS en het levert websites en activa aan browser van de cliënt
- Het plaatst verzoeken om tegen een veel sneller tarief opnieuw te dienen dan een AEM server op zijn kon uitvoeren
- Veel meer...

## Workflow voor webverkeer

Als u begrijpt welke onderdelen samen zijn geïnstalleerd om een eenvoudige Dispatcher-server te bouwen, krijgt u meer inzicht in de basisworkflow voor webverkeer voor een configuratie met Adobe Manager Services.
Dit zou u moeten helpen begrijpen welke rol het in de ketting van systemen speelt die inhoud aan bezoekers van uw AEM inhoud dienen.

<b> het dienen reeds caching inhoud </b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b> Serving verse inhoud van AEM </b>

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

<b> Inhoud publiceren/veranderingen </b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[Volgende -> Basisbestandsindeling](./basic-file-layout.md)
