---
title: IntelliJ instellen met het gereedschap Repo
description: Uw IntelliJ voorbereiden om te synchroniseren met AEM cloudinstantie
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 2%

---

# Cygwin installeren


Cygwin is een met POSIX compatibele programmeeromgeving en runtimeomgeving die native op Microsoft Windows wordt uitgevoerd.
Installeren [Cygwin](https://www.cygwin.com/). Ik ben geïnstalleerd in C:\cygwin64 folder
>[!NOTE]
> Zorg ervoor dat u ZIP-, unzip-, curl- en synchronisatiepakketten installeert met uw cygwin-installatie

Maak een map met de naam adoberepo onder de map c:\cloudmanager.

[Het gereedschap Repo installeren].(https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo).Installing het gereedschap Repo is niets anders dan het repobestand kopiëren en in uw c:\cloudmanger\adoberepo folder plaatsen.

Voeg het volgende toe aan de omgevingsvariabele Pad C:\cygwin64\bin;C:\CloudManager\adoberepo;

## Externe gereedschappen instellen

* Start IntelliJ
* Druk op Ctrl+Alt+S om het instellingenvenster te openen.
* Selecteer Gereedschappen->Externe gereedschappen, klik op het plusteken (+) en voer het volgende in zoals in de schermafbeelding.
   ![rep](assets/repo.png)
* Zorg ervoor dat u een groep met de naam Repo maakt door &quot;repo&quot; te typen in het vervolgkeuzemenu Groep en alle opdrachten die u maakt, behoren tot de opdracht **repo** groep


**Opdracht plaatsen**
**Programma**: C:\cygwin64\bin\bash
**Argumenten**: -l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**Werkmap**: \$ProjectFileDir\$
![put-command](assets/put-command.png)

**Opdracht ophalen**
**Programma**: C:\cygwin64\bin\bash
**Argumenten**: -l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**Werkmap**: \$ProjectFileDir\$
![get-command](assets/get-command.png)

**Status, opdracht**
**Programma**: C:\cygwin64\bin\bash
**Argumenten**: -l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**Werkmap**: \$ProjectFileDir\$
![status-command](assets/status-command.png)

**Diff-opdracht**
**Programma**: C:\cygwin64\bin\bash
**Argumenten**: -l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**Werkmap**: \$ProjectFileDir\$
![diff-opdracht](assets/diff-command.png)

Het .repo-bestand extraheren uit [repo.zip](assets/repo.zip) en plaatst het in de hoofdmap van uw AEM projecten. (C:\CloudManager\aem-banking-application). Open het .repo-bestand en zorg ervoor dat de server en de aanmeldingsinstellingen overeenkomen met uw omgeving.
Open het .gitignore-bestand en voeg het volgende toe onder aan het bestand en sla de wijzigingen \# repo.repo op

Selecteer elk project in uw em-bank-toepassingsproject, zoals ui.content en klik met de rechtermuisknop, en u ziet de optie Repo en onder de optie Repo ziet u de vier opdrachten die we eerder hebben toegevoegd.

## AEM-auteurinstantie instellen

U kunt de volgende stappen volgen om snel een cloudinstantie op uw lokale systeem in te stellen.
* [Download de nieuwste AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [Download de nieuwste AEM Forms addon](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* De volgende mapstructuur c:\aemformscs\aem-sdk\author maken

* Extraheer het bestand aem-sdk-quickstart-xxxxxxxxx.jar uit het ZIP-bestand van de AEM SDK en plaats het bestand in de map c:\aemformscs\aem-sdk\author folder.Rename.

* Open de opdrachtprompt en navigeer naar c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui. Hiermee wordt de installatie van AEM gestart.
* Aanmelden met admin/admin-referenties
* De AEM-instantie stoppen
* Maak de volgende mapstructuur.C:\aemformscs\aem-sdk\author\crx-quickstart\install
* Kopieer aem-forms-addon-xxxxxx.far in installatiemap
* Open de opdrachtprompt en navigeer naar c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui. Hiermee implementeert u de formulieren die u toevoegt aan het pakket in uw AEM.
