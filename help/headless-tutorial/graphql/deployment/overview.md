---
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---



# Implementaties zonder kop AEM

AEM clientimplementaties zonder kop zijn in vele vormen te vinden; SPA die door AEM worden gehost, externe SPA of website, mobiele app of zelfs server-naar-server proces.

Afhankelijk van de client en hoe deze wordt geïmplementeerd, hebben AEM headless-implementaties verschillende overwegingen.

## AEM

Voorafgaand aan het onderzoeken van plaatsingsoverwegingen, is het noodzakelijk om AEM logische architectuur, en de scheiding en de rollen van AEM as a Cloud Service de dienstrijen te begrijpen. AEM as a Cloud Service bestaat uit twee logische diensten:

+ __AEM-auteur__ is de dienst waar de teams tot stand brengen, samenwerken en de Fragments van de Inhoud (en andere activa) publiceren
+ __AEM-publicatie__ is de dienst die waar gepubliceerde Inhoudsfragmenten (en andere activa) voor algemeen gebruik worden herhaald

AEM Koploze klanten die in een productiecapaciteit werken, communiceren doorgaans met AEM Publish, dat de goedgekeurde, gepubliceerde inhoud bevat. Bij interactie van de client met AEM-auteur moet speciale aandacht worden besteed aan het feit dat de AEM-auteur standaard veilig is, waarvoor toestemming voor alle aanvragen vereist is, en dat deze onvolledige of niet-goedgekeurde inhoud kan bevatten.

## Hoofdloze clientimplementaties

+ [App met één pagina](./spa.md)
+ [Webcomponent/JS](./web-component.md)
+ [Mobiele app](./mobile.md)
+ [Server naar server](./server-to-server.md)

