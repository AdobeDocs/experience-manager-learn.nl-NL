---
title: AEM extensies van de inhoudsfragmentconsole
description: Leer hoe u AEM extensies voor de as a Cloud Service inhoudsfragmentconsole bouwt en implementeert
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
source-git-commit: fbc8c11841f5b5e04a99ba74fac6f01dc3e3a2da
workflow-type: tm+mt
source-wordcount: '742'
ht-degree: 0%

---


# Consoleversie AEM Content Fragments

[Console AEM inhoudsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) extensies kunnen worden toegevoegd via twee extensiepunten: een knop in het dialoogvenster [Inhoudsfragmentconsole](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) koptekstmenu of actiebalk. De extensies worden geschreven in JavaScript en worden uitgevoerd als App Builder-apps. U kunt ook een aangepaste webinterface en serverloze Adobe I/O Runtime-handelingen implementeren om intensieve, langlopende werkzaamheden uit te voeren.

![Consoleversie AEM Content Fragments](./assets/overview/example.png){align="center"}

| Extensietype | Beschrijving | Parameter(s) |
| :--- | :--- | :--- |
| Menu Koptekst | Hiermee voegt u een knop toe aan de koptekst die wordt weergegeven wanneer __nul__ Inhoudsfragmenten worden geselecteerd. | Geen. |
| Actiebalk | Hiermee wordt een knop toegevoegd aan de actiebalk die wordt weergegeven wanneer __een of meer__ Inhoudsfragmenten worden geselecteerd. | Een array van de paden van de geselecteerde inhoudsfragmenten. |

Een enkele AEM extensie van de Console van inhoudsfragmenten kan nul of één koptekstmenu en nul of één extensietype van de actiebalk bevatten. Als meerdere extensietypen van hetzelfde type vereist zijn, moeten er meerdere AEM extensies van de Content Fragments Console worden gemaakt.

AEM de uitbreidingen van de Console van Inhoudsfragmenten, vereisen en [Adobe Developer Console-project](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) en [App Builder-app](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) met de `@adobe/aem-cf-admin-ui-ext-tpl` sjabloon, gekoppeld aan het Adobe Developer Console-project.

Maak een keuze uit de volgende mogelijkheden wanneer u de App Builder-app genereert, op basis van wat de extensie doet. Elke combinatie van opties kan in een extensie worden gebruikt.

|  | Knop Toevoegen aan [Menu Koptekst](./header-menu.md) | Knop Toevoegen aan [Actiebalk](./action-bar.md) | Tonen [Modal](./modal.md) | Toevoegen [server-side handler](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| Beschikbaar wanneer Inhoudsfragmenten niet zijn geselecteerd | ✔ |  |  |  |
| Beschikbaar wanneer een of meer inhoudsfragmenten zijn geselecteerd |  | ✔ |  |  |
| Hiermee wordt aangepaste invoer van de gebruiker verzameld |  |  | ✔️ |  |
| Aangepaste feedback weergeven |  |  | ✔️ |  |
| Roept HTTP-aanvragen aan om te AEM |  |  |  | ✔ |
| Roept HTTP-verzoeken aan Adobe/services van derden aan |  |  |  | ✔ |


## Adobe Developer-documentatie

Adobe Developer bevat ontwikkelaarsgegevens voor AEM extensies van Content Fragment Console. Controleer de [Adobe Developer-inhoud voor verdere technische details](https://developer.adobe.com/uix/docs/).

## Een extensie ontwikkelen

Volg de onderstaande stappen om te leren hoe u een extensie voor AEM inhoudsfragmentconsole voor AEM as a Cloud Service kunt genereren, ontwikkelen en implementeren.

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./adobe-developer-console-project.md" title="Adobe Developer-project maken" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="Adobe Developer-project maken">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1. Een project maken</p>
                    <p class="is-size-6">Maak een Adobe Developer Console-project waarin de toegang tot andere Adobe-services wordt gedefinieerd en waarin de implementaties worden beheerd.</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Een Adobe Developer-project maken</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Generate an Extension app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Generate an Extension app">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./app-initialization.md" title="Een extensie-app genereren" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="Een extensie-app initialiseren">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2. Een extensie-app initialiseren</p>
                    <p class="is-size-6">Initialiseer een app voor de extensie App Builder van een AEM Content Fragment Console die definieert waar de extensie wordt weergegeven en welk werk er wordt uitgevoerd.</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Een extensie-app initialiseren</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension registration -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension registration">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./extension-registration.md" title="Registratie van extensies" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="Registratie van extensies">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3. Registratie van extensies</p>
                    <p class="is-size-6">Registreer de extensie in de AEM Content Fragment Console als een headermenu of extensietype van de actiebalk.</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">De extensie registreren</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Header Menu -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./header-menu.md" title="Menu Koptekst" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="Menu Koptekst">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4 bis. Menu Koptekst</p>
                    <p class="is-size-6">Leer hoe u een extensie van het kopmenu van de AEM Content Fragment Console maakt.</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Het kopmenu uitbreiden</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Action Bar -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action Bar">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./action-bar.md" title="Actiebalk" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/action-bar/card.png" alt="Actiebalk">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4 ter. Actiebalk</p>
                    <p class="is-size-6">Leer hoe u een extensie voor de actiebalk van de AEM Content Fragment Console maakt.</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">De actiebalk uitbreiden</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Modal -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modal">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./modal.md" title="Modal" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/modal/card.png" alt="Modal">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">5. Modal</p>
                    <p class="is-size-6">Voeg een aangepaste modaal aan de uitbreiding toe die kan worden gebruikt om op maat gemaakte ervaringen voor gebruikers tot stand te brengen. De modules verzamelen vaak input van gebruikers, en tonen de resultaten van een verrichting.</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Een modaal object toevoegen</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Adobe I/O Runtime action -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe I/O Runtime action">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./runtime-action.md" title="Adobe I/O Runtime-actie" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Adobe I/O Runtime-actie">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6. Adobe I/O Runtime-actie</p>
                    <p class="is-size-6">Voeg een serverloze Adobe I/O Runtime-actie toe die de extensie kan aanroepen om te communiceren met Content Fragments en AEM om aangepaste zakelijke bewerkingen uit te voeren.</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Een Adobe I/O Runtime-handeling toevoegen</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Test -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Test">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./test.md" title="Testen" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/test/card.png" alt="Testen">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">7. Testen</p>
                    <p class="is-size-6">Test de extensies tijdens de ontwikkeling en deel voltooide extensies met een speciale URL voor QA- of UAT-testers.</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">De extensie testen</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension deployment -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension deployment">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./deploy.md" title="Extensie-implementatie" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="Extensie-implementatie">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8. Implementatie van productie</p>
                    <p class="is-size-6">Implementeer de extensie om deze beschikbaar te maken voor AEM gebruikers. Extensies kunnen worden bijgewerkt en ook worden verwijderd.</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Distribueren naar productie</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## Voorbeelden van extensies

Voorbeeld AEM extensies van Content Fragment Console.

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="Extensie voor update van eigenschap Bulk" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="Extensie voor update van eigenschap Bulk">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Extensie voor update van eigenschap Bulk</p>
                    <p class="is-size-6">Verken een extensie van een voorbeeldactiebalk waarmee bulksgewijs een eigenschap voor geselecteerde inhoudsfragmenten wordt bijgewerkt.</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">De voorbeeldextensie verkennen</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="Afbeeldingen genereren en uploaden naar AEM extensie" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="Afbeeldingen genereren en uploaden naar AEM extensie">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Afbeeldingen genereren en uploaden naar AEM extensie</p>
                    <p class="is-size-6">Onderzoek een uitbreiding van de voorbeeldactie die een beeld gebruikend OpenAI produceert, uploadt het aan AEM en werkt beeldbezit op het geselecteerde Inhoudsfragment bij.</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">De voorbeeldextensie verkennen</span>
                    </a>
                </div>
            </div>
        </div>
    </div>



</div>
