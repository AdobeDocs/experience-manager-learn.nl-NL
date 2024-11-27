---
title: De structuur voor de landencomponent maken
description: De componentstructuur in crx maken
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 1%

---

# De structuur voor de landencomponent maken

Meld u aan bij uw AEM Forms-exemplaar en voer de volgende stappen uit om een nieuwe component te maken op basis van de vervolgkeuzelijst buiten de doos:

1. Navigeer naar &#39;/apps/&lt;yourproject>/components/adaptiveForm/dropdown&#39; in CRXDE Lite.
2. Kopieer de vervolgkeuzelijst en plak deze op hetzelfde mapniveau.
3. Wijzig de naam van de gekopieerde component in landen.
4. Werk de eigenschap jcr:title van het cq:sjabloonknooppunt bij naar Landen.
5. Sla de wijzigingen op.

Er is nu een nieuwe component met de naam Landen. Dit is een exacte replica van de vervolgkeuzelijst buiten de doos. Dit is de basis voor verdere aanpassing.

## Het HTML-bestand maken

U kunt als volgt het HTML-bestand voor de component Landen maken:

1. Navigeer naar de map countries in de crx-opslagplaats
2. Maak een nieuw bestand met de naam countries.html.
3. Open het bestand /apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html in de crx-opslagplaats en kopieer de inhoud ervan.
4. Plak de gekopieerde inhoud in countries.html.
5. Wijzig de code om het nieuwe tekenmodel te gebruiken, zoals weergegeven in de schermafbeelding
6. Sla uw wijzigingen op.

![ sling-model ](assets/countriesdropdown.png)

Tot slot synchroniseert u uw project met deze updates om ervoor te zorgen dat de wijzigingen in de CRX-opslagplaats worden weerspiegeld in uw AEM project.


## Volgende stappen

[CQ-dialoogvenster maken](./dialog.md)
