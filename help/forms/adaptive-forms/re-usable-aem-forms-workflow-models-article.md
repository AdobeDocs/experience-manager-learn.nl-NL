---
title: Opnieuw bruikbare AEM Forms Workflow-modellen
description: Leer hoe u workflowmodellen maakt die onafhankelijk zijn van Adaptive Forms.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---

# Herbruikbare AEM Forms-workflowmodellen maken{#create-re-usable-aem-forms-workflow-models}

Vanaf AEM Forms 6.5 kunnen we nu workflowmodellen maken die niet zijn gekoppeld aan een specifiek adaptief formulier. Met deze functie kunt u nu één workflowmodel maken dat kan worden aangeroepen voor verschillende adaptieve formulierverzendingen. Met deze functie kunt u een algemene workflow gebruiken voor het verwerken van alle aangepaste formulierverzendingen voor revisie en goedkeuring.

Voer de volgende stappen uit om een dergelijke workflow te ontwerpen

1. Aanmelden bij AEM
1. Wijs uw browser aan [workflowmodel](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Klikken __Maken > Model maken__ om een workflowmodel toe te voegen
1. Geef de juiste naam en titel op voor het workflowmodel en klik op Gereed
1. Het nieuwe model openen in de bewerkingsmodus
1. De belemmering en laat vallen wijst de component van de Taak op uw werkschemamodel toe
1. Open de de configuratieeigenschappen van de component van de Taak toewijzen
1. Tab naar het tabblad Forms en Documenten
1. Selecteer het type Adaptief formulier of Alleen-lezen adaptief formulier.

Er zijn drie manieren waarop het formulierpad kan worden opgegeven

1. Beschikbaar op een absoluut pad - Dit betekent dat de workflow nauw gekoppeld is aan een adaptief formulier. Dat is niet wat wij hier willen
1. **Verzonden naar de workflow** - Dit betekent dat wanneer het adaptieve formulier wordt ingediend, de workflow-engine de naam van het formulier uit de ingediende gegevens haalt. Dit is de optie die moet worden geselecteerd
1. Beschikbaar op een pad in een variabele - Dit betekent dat het adaptieve formulier wordt opgehaald uit de workflowvariabele. In de volgende screenshot ziet u de juiste optie die u moet kiezen voor ontkoppelde workflow in een adaptief formulier

![Opnieuw bruikbare AEM Forms Workflow-modellen](assets/workflomodel.PNG)
