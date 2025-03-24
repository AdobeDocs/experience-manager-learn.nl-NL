---
title: Installeren en instellen
description: Initialiseer uw lokale git-opslagplaats
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Installatiegit


[ installeer Git ](https://git-scm.com/downloads). U kunt de standaardinstellingen selecteren en het installatieproces voltooien.
Ga naar de opdrachtprompt
Ga naar c:\cloudmanager\aem-banking-app
type in git —version. U moet de versie van de GIT zien die op uw systeem is geïnstalleerd

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
![ toegang tot rep info ](assets/cloud-manager-repo.png)
Geef de repo-gegevens van de cloud Manager op
![ get-credentials ](assets/cloud-manager-repo1.png)

De gebruikersnaam opslaan in het configuratiebestand

```java
git config --global credential.username "gbedekar-adobe-com"
```

het wachtwoord opslaan in het configuratiebestand

```java
git config --global user.password "XXXX"
```

(Het wachtwoord is het wachtwoord voor de gegevensopslagruimte van uw cloudmanager)

Registreer de gegevensopslagruimte van de cloudmanager bij uw lokale opslagplaats voor it. Het bevel hieronder associeert **bankingapp** met de verre bewaarplaats van de wolkenmanager. U zou om het even welke naam in plaats van **bankingapp** kunnen gebruiken


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

(Controleer of u de URL van de opslagplaats gebruikt)

Controleren of de externe opslagplaats is geregistreerd

```java
git remote -v
```

## Volgende stappen

[AEM synchroniseren met AEM Project in IntelliJ](./intellij-and-aem-sync.md)
