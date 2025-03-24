---
title: Wat is "De Dispatcher"
description: Begrijp wat een Dispatcher eigenlijk is.
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 73
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
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




## AEM specific module file (`mod_dispatcher.so`)

Voeg vervolgens een plug-in toe aan de Apache Web Server, de zogenaamde Dispatcher-module

Basisuitleg van wat de Adobe AEM Dispatcher module doet:

- Hiermee wordt de standaardbestandshandler versterkt
- Filters verzenden slechte aanvragen/beschermen zachte buik/eindpunten van AEM
- Taakbalansen als er meer dan één renderer aanwezig is
- Staat voor een levende geheim voorgeheugenfolder/steunt het spoelen van stagnerende dossiers toe
- Het is de voordeur voor alle installaties van AMS en het levert websites en activa aan browser van de cliënt
- Het plaatst verzoeken om tegen een veel sneller tarief opnieuw te dienen dan een server van AEM op zich kon verwezenlijken
- Veel meer...

## Workflow voor webverkeer

Als u begrijpt welke onderdelen samen zijn geïnstalleerd om een eenvoudige Dispatcher-server te bouwen, krijgt u meer inzicht in de basisworkflow voor webverkeer voor een Adobe Manager Services-configuratie.
Dit zou u moeten helpen begrijpen welke rol het in de ketting van systemen speelt die inhoud aan bezoekers van uw inhoud van AEM dienen.

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
