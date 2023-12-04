---
title: Geavanceerde netwerken
description: Leer over AEM as a Cloud Service geavanceerde voorzien van een netwerkopties.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
duration: 157
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---

# Geavanceerde netwerken

AEM as a Cloud Service verstrekt geavanceerde voorzien van een netwerkeigenschappen die voor nauwkeurige beheer van verbindingen aan en van AEM as a Cloud Service programma&#39;s toestaan.

|                                                   | [Productieprogramma&#39;s](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [Sandbox-programma&#39;s](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Ondersteunt geavanceerd netwerken | ✔ | ✘ |


AEM geavanceerde voorzien van een netwerk wordt samengesteld uit drie opties om connectiviteit met de externe diensten te beheren. Een programma van de Manager van de Wolk, en zijn AEM as a Cloud Service milieu&#39;s, kunnen slechts één enkel type van geavanceerde voorzien van een netwerkconfiguratie tegelijkertijd gebruiken, zodat ervoor zorgt dat het meest aangewezen type wordt geselecteerd.

|                                   | HTTP/HTTPS op standaardpoorten | HTTP/HTTPS op niet-standaardpoorten | Niet-HTTP/HTTPS-verbindingen | Speciale IP-adressen | Lijst met &quot;Geen proxy-hosts&quot; | Verbinding maken met VPN-beveiligde services | Beperk AEM publicatieverkeer door IP |
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

## Codevoorbeelden

Deze inzameling verstrekt voorbeelden van de configuratie en de code die aan hefboomwerking geavanceerde voorzien van een netwerkeigenschappen voor specifieke gebruiksgevallen wordt vereist.

Zorgen voor de juiste [geavanceerde netwerkconfiguratie](#advanced-networking) is ingesteld voordat deze zelfstudies zijn uitgevoerd.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtual Private Network (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">E-mailservice</a></strong></div>
      <p>
        OSGi configuratievoorbeeld dat AEM gebruikt om met externe e-maildiensten te verbinden.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Java™-codevoorbeeld waarbij een HTTP/HTTPS-verbinding van AEM as a Cloud Service wordt gemaakt met een externe service via het HTTP/HTTPS-protocol.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="SQL-verbinding met JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">SQL-verbinding met JDBC DataSourcePool</a></strong></div>
      <p>
            Java™ codevoorbeeld die met externe SQL gegevensbestanden verbinden door AEM JDBC datasource pool te vormen.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="SQL-verbinding met Java API&apos;s" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">SQL-verbinding met Java™ API's</a></strong></div>
      <p>
            Java™-codevoorbeeld voor verbinding met externe SQL-databases met SQL API's van Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="Een IP-lijst van gewenste personen toepassen" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Een IP lijst van gewenste personen toepassen</a></strong></div>
      <p>
            Vorm een IP lijst van gewenste personen dusdanig dat slechts het verkeer van VPN tot AEM kan toegang hebben.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="Op pad gebaseerde VPN-toegangsbeperkingen voor AEM publiceren" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">Op pad gebaseerde VPN-toegangsbeperkingen voor AEM publiceren</a></strong></div>
      <p>
            Vereis de toegang van VPN voor specifieke wegen op AEM publiceren.
      </p>
    </td>
</tr>
</table>
