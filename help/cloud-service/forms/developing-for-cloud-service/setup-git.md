---
title: Installeren en instellen
description: Initialiseer uw lokale git-opslagplaats
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
duration: 48
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Installatiegit


[Installatiegit](https://git-scm.com/downloads). U kunt de standaardinstellingen selecteren en het installatieproces voltooien.
Ga naar de opdrachtprompt Navigeer naar c:\cloudmanager\aem-banking-app type in git —version. U moet de versie van de GIT zien die op uw systeem is geïnstalleerd

## Opslagplaats lokale it initialiseren

Zorg ervoor dat u in de map c:\cloudmanager\aem-banking-app bent

```
git init
```

Het bovenstaande bevel zal het project als git lokale bewaarplaats initialiseren

```
git add .
```

Hiermee worden alle projectbestanden toegevoegd aan de it-opslagplaats die klaar zijn om te worden toegewezen aan de git-opslagplaats

```
git commit -m "initial commit"
```

Hierdoor worden de bestanden toegewezen aan de git-opslagplaats



## Registreer de cloudbeheeropslagplaats bij onze lokale Git-opslagplaats

Toegang tot het rapport van de cloud Manager
![toegang krijgen tot de rep info](assets/cloud-manager-repo.png)
Geef de repo-gegevens van de cloud Manager op
![getCredits](assets/cloud-manager-repo1.png)

De gebruikersnaam opslaan in het configuratiebestand

```java
git config --global credential.username "gbedekar-adobe-com"
```

het wachtwoord opslaan in het configuratiebestand

```java
git config --global user.password "XXXX"
```

(Het wachtwoord is het wachtwoord voor de gegevensopslagruimte van uw cloudmanager)

Registreer de gegevensopslagruimte van de cloudmanager bij uw lokale opslagplaats voor it. De onderstaande opdracht is gekoppeld **bankapp** met de externe cloudbeheeropslagplaats. U had een naam kunnen gebruiken in plaats van **bankapp**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

(Controleer of u de URL van de opslagplaats gebruikt)

Controleren of de externe opslagplaats is geregistreerd

```java
git remote -v
```

## Volgende stappen

[AEM synchroniseren met AEM project in IntelliJ](./intellij-and-aem-sync.md)
