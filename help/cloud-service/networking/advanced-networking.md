---
title: Geavanceerde netwerken
description: Leer over AEM as a Cloud Service geavanceerde voorzien van een netwerkopties.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Geavanceerde netwerken

AEM as a Cloud Service verstrekt drie opties om connectiviteit met de externe diensten te beheren. Een programma van de Manager van de Wolk, en zijn AEM as a Cloud Service milieu&#39;s, kunnen slechts één enkel type van geavanceerde voorzien van een netwerkconfiguratie tegelijkertijd gebruiken, zodat ervoor zorgt dat het meest aangewezen type wordt geselecteerd.

|  | HTTP/HTTPS op standaardpoorten | HTTP/HTTPS op niet-standaardpoorten | Niet-HTTP/HTTPS-verbindingen | Speciale IP-adressen | Lijst met &quot;Geen proxy-hosts&quot; | Verbinding maken met VPN-beveiligde services | Beperk AEM publicatieverkeer door IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Geen geavanceerde netwerken__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Flexibele poortuitgang__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__IP-adres van specifiek egress__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__Virtueel privé netwerk__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


Voor meer details over de betrokken overwegingen wanneer het selecteren van het aangewezen geavanceerde voorzien van een netwerktype, zie [geavanceerde netwerkdocumentatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## Geavanceerde zelfstudies voor netwerken

Zodra de meest aangewezen geavanceerde voorzien van een netwerkoptie die op de behoefte van uw organisatie wordt gebaseerd is geïdentificeerd, klik in het overeenkomstige leerprogramma hieronder aan voor geleidelijke instructies en codesteekproeven.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Flexibele poortuitgang" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Flexibele poortuitgang</a></strong></div>
      <p>
          Sta uitgaand AEM as a Cloud Service verkeer op niet-standaardhavens toe.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="IP-adres van FileDedicated egress" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">IP-adres van specifiek egress</a></strong></div>
      <p>
        Oorsprong uitgaand AEM as a Cloud Service verkeer van specifieke IP.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Virtual Private Network (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">Virtual Private Network (VPN)</a></strong></div>
      <p>
        Beveilig verkeer tussen een klant of verkopersinfrastructuur en AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>