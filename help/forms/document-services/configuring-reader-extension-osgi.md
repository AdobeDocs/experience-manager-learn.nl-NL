---
title: Reader-extensies configureren in AEM Forms OSGi
description: Voeg de credentie van de Uitbreidingen van de Reader aan de vertrouwensopslag in AEM Forms OSGi toe
feature: Reader-extensies
feature-set: Reader Extensions
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: Beheer
role: Admin
level: Beginner
source-git-commit: 55a6ff5d01898b994aee60f214126c5c18a06a5e
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---


# Credentials voor extensies voor Readers toevoegen{#configuring-reader-extension-osgi}

De DocAssurance-service kan gebruiksrechten toepassen op PDF-documenten. Als u gebruiksrechten wilt toepassen op PDF-documenten, configureert u de certificaten.

## Keystore maken voor Ffd-service-gebruiker

De referentie voor de lezerextensies is gekoppeld aan de gebruiker van de fd-service. Voer de volgende stappen uit om de referentie aan de gebruiker van de fd-service toe te voegen. Als u reeds keystore voor de fd-dienst gebruiker hebt gecreeerd sla deze sectie over

* Aanmelden bij de instantie van AEM-auteur als beheerder
* Ga naar Tools-Security-Users
* Schuif omlaag in de lijst met gebruikers totdat u een gebruikersaccount voor fd-service vindt
* Klik op de gebruiker van de fd-dienst
* Klik op het tabblad sleutelarchief
* Klik op Create KeyStore
* Stel het wachtwoord voor toegang tot KeyStore in en sla uw instellingen op om het wachtwoord voor KeyStore te maken

### Credentials toevoegen aan het sleutelarchief van de fd-service-gebruiker

Volg de video om de referenties aan de gebruiker van de fd-service toe te voegen

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)











