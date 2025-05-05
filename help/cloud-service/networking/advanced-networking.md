---
title: Geavanceerde netwerken
description: Meer informatie over geavanceerde netwerkopties van AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
duration: 85
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---

# Geavanceerde netwerken

AEM as a Cloud Service biedt geavanceerde netwerkfuncties waarmee u verbindingen met en van AEM as a Cloud Service-programma&#39;s nauwkeurig kunt beheren.

|                                                   | [ Programma&#39;s van de Productie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html?lang=nl-NL) | [Sandbox-programma&#39;s](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=nl-NL) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Ondersteunt geavanceerd netwerken | ✔ | ✘ |


Geavanceerde netwerken van AEM bestaan uit drie opties voor het beheer van connectiviteit met externe services. Een Cloud Manager-programma en zijn AEM as a Cloud Service-omgevingen kunnen slechts één type geavanceerde netwerkconfiguratie tegelijk gebruiken, zodat het meest geschikte type is geselecteerd.

|                                   | HTTP/HTTPS op standaardpoorten | HTTP/HTTPS op niet-standaardpoorten | Niet-HTTP/HTTPS-verbindingen | Speciale IP-adressen | Lijst met &quot;Geen proxy-hosts&quot; | Verbinding maken met VPN-beveiligde services | AEM-publicatieverkeer beperken tot IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Geen geavanceerd voorzien van een netwerk__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Flexibele havenuitgang__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__Dedicated egress IP adres__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__Virtueel Privé Netwerk__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


Voor meer details op de betrokken overwegingen wanneer het selecteren van het aangewezen geavanceerde voorzien van een netwerktype, zie [ geavanceerde voorzien van een netwerkdocumentatie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html?lang=nl-NL).

## Geavanceerde zelfstudies voor netwerken

Zodra de meest aangewezen geavanceerde voorzien van een netwerkoptie die op de behoefte van uw organisatie wordt gebaseerd is geïdentificeerd, klik in het overeenkomstige leerprogramma hieronder aan voor geleidelijke instructies en codesteekproeven.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Flexibele poortuitgang" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md"> Flexibele havenuitgang </a></strong></div>
      <p>
          Sta uitgaand verkeer van AEM as a Cloud Service op niet-standaardhavens toe.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="IP-adres van FileDedicated egress" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md"> Dedicated egress IP adres </a></strong></div>
      <p>
        Oorsprong uitgaand verkeer van AEM as a Cloud Service van specifieke IP.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Virtual Private Network (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md"> Virtueel Privé Netwerk (VPN) </a></strong></div>
      <p>
        Beveilig verkeer tussen een klant of verkopersinfrastructuur en AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>

## Codevoorbeelden

Deze inzameling verstrekt voorbeelden van de configuratie en de code die aan hefboomwerking geavanceerde voorzien van een netwerkeigenschappen voor specifieke gebruiksgevallen wordt vereist.

Verzeker de aangewezen [ geavanceerde voorzien van een netwerkconfiguratie ](#advanced-networking) opstelling voorafgaand aan het volgen van deze leerprogramma&#39;s is geweest.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtual Private Network (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md"> E-maildienst </a></strong></div>
      <p>
        OSGi configuratievoorbeeld dat AEM gebruikt om met externe e-maildiensten te verbinden.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md"> HTTP/HTTPS </a></strong></div>
        <p>
            Java™-codevoorbeeld waarbij via het HTTP/HTTPS-protocol een HTTP/HTTPS-verbinding van AEM as a Cloud Service naar een externe service wordt gemaakt.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="SQL-verbinding met JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md"> SQL verbinding gebruikend JDBC DataSourcePool </a></strong></div>
      <p>
            Java™ codevoorbeeld die met externe SQL gegevensbestanden verbinden door AEM te vormen JDBC datasource pool.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="SQL-verbinding met Java API&apos;s" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md"> SQL verbinding gebruikend Java™ APIs </a></strong></div>
      <p>
            Java™-codevoorbeeld voor verbinding met externe SQL-databases met SQL API's van Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=nl-NL"><img alt="Een IP-lijst van gewenste personen toepassen" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=nl-NL"> Toepassend een IP lijst van gewenste personen </a></strong></div>
      <p>
            Vorm een IP lijst van gewenste personen dusdanig dat slechts het verkeer van VPN tot AEM kan toegang hebben.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html?lang=nl-NL#restrict-vpn-to-ingress-connections"><img alt="Op pad gebaseerde VPN-toegangsbeperkingen voor AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html?lang=nl-NL#restrict-vpn-to-ingress-connections"> Op weg-Gebaseerde de toegangsbeperkingen van VPN aan AEM publiceren </a></strong></div>
      <p>
            Vereis de toegang van VPN voor specifieke wegen op AEM publiceren.
      </p>
    </td>
</tr>
</table>
