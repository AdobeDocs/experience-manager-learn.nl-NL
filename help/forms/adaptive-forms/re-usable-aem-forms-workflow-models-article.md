---
title: Opnieuw bruikbare AEM Forms Workflow-modellen
description: Leer hoe u workflowmodellen maakt die onafhankelijk zijn van Adaptive Forms.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
last-substantial-update: 2020-06-09T00:00:00Z
duration: 58
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Herbruikbare AEM Forms-workflowmodellen maken{#create-re-usable-aem-forms-workflow-models}

Vanaf AEM Forms 6.5 kunnen we nu workflowmodellen maken die niet zijn gekoppeld aan een specifiek adaptief formulier. Met deze functie kunt u nu één workflowmodel maken dat kan worden aangeroepen voor verschillende adaptieve formulierverzendingen. Met deze functie kunt u een algemene workflow gebruiken voor het verwerken van alle adaptieve formulierverzendingen voor revisie en goedkeuring.

Voer de volgende stappen uit om een dergelijke workflow te ontwerpen

1. Aanmelden bij AEM
1. Punt uw browser aan [ werkschemamodel ](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Klik __creëren > Model__ creëren om een werkschemamodel toe te voegen
1. Geef de juiste naam en titel op voor het workflowmodel en klik op Gereed
1. Het nieuwe model openen in de bewerkingsmodus
1. Sleep de taakcomponent toewijzen aan het workflowmodel en zet deze neer
1. Open de de configuratieeigenschappen van de component van de Taak toewijzen
1. Tabblad Forms en Documenten
1. Selecteer het type Adaptief formulier of Alleen-lezen adaptief formulier.

Het formulierpad kan op drie manieren worden opgegeven

1. Beschikbaar op een absoluut pad - Dit betekent dat de workflow nauw gekoppeld is aan een adaptief formulier. Dat is niet wat wij hier willen
1. **Voorgelegd aan het werkschema** - dit betekent wanneer de adaptieve vorm wordt voorgelegd, haalt de werkschemamotor de naam van de vorm uit de voorgelegde gegevens. Dit is de optie die moet worden geselecteerd
1. Beschikbaar op een pad in een variabele - Dit betekent dat het aangepaste formulier wordt opgehaald uit de variabele met de workflow.
In de volgende schermafbeelding ziet u de juiste optie die u moet kiezen voor de ontkoppelingsworkflow in een adaptief formulier

![ herbruikbare modellen van het Werkschema van AEM Forms ](assets/workflomodel.PNG)
