---
title: Reader-extensies configureren in AEM Forms OSGi
description: Reader Extensions-referentie toevoegen aan de vertrouwde opslag in AEM Forms OSGi
feature: Reader Extensions
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
duration: 308
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Reader Extensions-referentie toevoegen{#configuring-reader-extension-osgi}

De DocAssurance-service kan gebruiksrechten toepassen op PDF-documenten. Als u gebruiksrechten wilt toepassen op PDF-documenten, configureert u de certificaten.

## Keystore maken voor Ffd-service-gebruiker

De referentie voor de lezerextensies is gekoppeld aan de gebruiker van de fd-service. Voer de volgende stappen uit om de referentie aan de gebruiker van de fd-service toe te voegen. Als u reeds keystore voor de fd-dienst gebruiker hebt gecreeerd sla deze sectie over

* Aanmelden bij de AEM Author-instantie als beheerder
* Ga naar Tools-Security-Users
* Schuif omlaag in de lijst met gebruikers totdat u een gebruikersaccount voor fd-service vindt
* Klik op de gebruiker van de fd-dienst
* Klik op het tabblad sleutelarchief
* Klik op Create KeyStore
* Stel het wachtwoord voor toegang tot KeyStore in en sla uw instellingen op om het wachtwoord voor KeyStore te maken

### Credentials toevoegen aan het sleutelarchief van de fd-service-gebruiker

Volg de video om de referenties aan de gebruiker van de fd-service toe te voegen

>[!VIDEO](https://video.tv.adobe.com/v/3447293?quality=12&learn=on&captions=dut)


De opdracht voor het weergeven van de details van het pfx-bestand is. Bij de volgende opdracht wordt ervan uitgegaan dat u zich in dezelfde map bevindt als het pfx-bestand.

**keytool -v -list -storetype pkcs12 - keystore &lt;name van uw .pfx dossier>**

Bijvoorbeeld keytool -v -list -storetype pkcs12 -keystore 1005566.pfx waarbij 1005566.pfx de naam van mijn pfx-bestand is
