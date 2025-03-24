---
title: IntelliJ instellen met het gereedschap Repo
description: Uw IntelliJ voorbereiden op synchronisatie met AEM Cloud Ready-instantie
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
duration: 92
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---

# Cygwin installeren


Cygwin is een met POSIX compatibele programmeeromgeving en runtimeomgeving die native op Microsoft Windows wordt uitgevoerd.
Installeer [ Cygwin ](https://www.cygwin.com/). Ik ben geïnstalleerd in de map C:\cygwin64
>[!NOTE]
> Zorg ervoor dat u ZIP-, unzip-, curl- en synchronisatiepakketten installeert met uw cygwin-installatie

Maak een map met de naam adoberepo onder de map c:\cloudmanager.

[ installeer het repo hulpmiddel ](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo) Installerend het repo hulpmiddel is niets behalve het kopiëren van het repo dossier en het plaatsen van het in uw c: \ cloudmanger \ adoberepo omslag.

Voeg het volgende toe aan de omgevingsvariabele Pad C:\cygwin64\bin;C:\CloudManager\adoberepo;

## Externe gereedschappen instellen

* Start IntelliJ
* Druk op Ctrl+Alt+S om het instellingenvenster te openen.
* Selecteer Gereedschappen->Externe gereedschappen, klik op het plusteken (+) en voer het volgende in zoals in de schermafbeelding.
  ![ rep ](assets/repo.png)
* Zorg ervoor u een groep geroepen repo door in &quot;repo&quot;op het drop-down gebied van de Groep te typen en alle bevelen creeert die u tot de **repogroep** creeert


**Gezet Bevel**
**Programma**: C:\cygwin64\bin\bash
**Arguments**: -l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**Werkmap**: \$ProjectFileDir\$
![ plaats-bevel ](assets/put-command.png)

**krijgt Bevel**
**Programma**: C:\cygwin64\bin\bash
**Arguments**: -l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**Werkmap**: \$ProjectFileDir\$
![ get-command ](assets/get-command.png)

**Bevel van de Status**
**Programma**: C:\cygwin64\bin\bash
**Arguments**: -l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**Werkmap**: \$ProjectFileDir\$
![ status-bevel ](assets/status-command.png)

**Diff Bevel**
**Programma**: C:\cygwin64\bin\bash
**Arguments**: - l C:\CloudManager\adoberepo\repo diff - f $FilePath$
**Werkmap**: \$ProjectFileDir\$
![ diff-bevel ](assets/diff-command.png)

Extraheer het .repo- dossier van [ repo.zip ](assets/repo.zip) en plaats het in uw de projectwortelomslag van AEM. (C:\CloudManager\aem-banking-application) Open het .repo-bestand en zorg ervoor dat de server en de aanmeldingsinstellingen overeenkomen met uw omgeving.
Open het .gitignore-bestand en voeg het volgende toe onder aan het bestand en sla de wijzigingen op
\# repo
.repo

Selecteer elk project in uw em-bank-toepassingsproject, zoals ui.content en klik met de rechtermuisknop, en u ziet de optie Repo en onder de optie Repo ziet u de vier opdrachten die we eerder hebben toegevoegd.

## AEM Auteur-instantie instellen{#set-up-aem-author-instance}

U kunt de volgende stappen volgen om snel een cloudinstantie op uw lokale systeem in te stellen.
* [ Download recentste AEM SDK ](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [ Download de recentste addon van AEM Forms ](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* De volgende mapstructuur maken
c:\aemformscs\aem-sdk\author

* Extraheer het bestand aem-sdk-quickstart-xxxxxxxxx.jar uit het ZIP-bestand voor AEM SDK en plaats dit in de map c:\aemformscs\aem-sdk\author.Wijzig de naam van het jar-bestand in aem-auteur-p4502.jar

* Opdrachtprompt openen en naar c:\aemformscs\aem-sdk\author gaan
Voer de volgende opdracht java -jar aem-auteur-p4502.jar -gui in. Hiermee wordt de installatie van AEM gestart.
* Aanmelden met admin/admin-referenties
* De AEM-instantie stoppen
* Maak de volgende mapstructuur.C:\aemformscs\aem-sdk\author\crx-quickstart\install
* Kopieer aem-forms-addon-xxxxxx.far in installatiemap
* Opdrachtprompt openen en naar c:\aemformscs\aem-sdk\author gaan
Voer de volgende opdracht java -jar aem-auteur-p4502.jar -gui in. Hiermee worden de formulieren die u toevoegt in een pakket in uw AEM-exemplaar geïmplementeerd.

## Volgende stappen

[AEM-formulieren en -sjablonen synchroniseren met AEM-project](./deploy-your-first-form.md)
