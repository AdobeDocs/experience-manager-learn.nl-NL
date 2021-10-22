---
title: Installeren en instellen
description: Initialiseer uw lokale git-opslagplaats
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8848
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Installatiegit


[Installatiegit](https://git-scm.com/downloads). U kunt de standaardinstellingen selecteren en het installatieproces voltooien.
Ga naar de opdrachtprompt Ga naar c:\cloudmanager\aem-banking-app type in git —versie. U moet de versie van de GIT zien die op uw systeem is geïnstalleerd

## Opslagplaats lokale it initialiseren

Zorg ervoor u in c:\cloudmanager\aem-banking-app folder bent

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
![de rep info openen](assets/cloud-manager-repo.png)
Geef de repo-gegevens van de cloud Manager op
![getCredits](assets/cloud-manager-repo1.png)

De gebruikersnaam opslaan in het configuratiebestand

```java
git config --global credential.username "gbedekar-adobe-com"
```

het wachtwoord opslaan in het configuratiebestand

```java
git config --global user.password "bqwxfvxq2akawtqx3oztacb5wax5a7"
```

(Het wachtwoord is het wachtwoord voor de gegevensopslagruimte van uw cloudmanager)

Registreer de gegevensopslagruimte van de cloudmanager bij uw lokale opslagplaats voor it. De onderstaande opdracht is gekoppeld **adobe** met de externe cloudbeheeropslagplaats. U had een naam kunnen gebruiken in plaats van **adobe**


```java
git remote add adobe https://git.cloudmanager.adobe.com/techmarketingdemos/Program2-p24107/
```

(Controleer of u de URL van de opslagplaats gebruikt)

Controleren of de externe opslagplaats is geregistreerd

```java
git remote -v
```



