---
title: Verificatie in AEM as a Cloud Service
description: Leer over authentificatie in AEM as a Cloud Service's.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 10436
thumbnail: KT-10436.jpeg
source-git-commit: e666e38d6b2a7057f7016b35ad1034a4487e9bc7
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 0%

---


# as a Cloud Service verificatie AEM

AEM as a Cloud Service steunt veelvoudige van authentificatieopties en varieert door de diensttype.

|  | AEM-auteur | AEM-publicatie |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [Tokenverificatie](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |

## Verificatieopties

Klik in de overeenkomstige verbinding hieronder aan voor details over opstelling en gebruik de authentificatiebenadering.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Toegang tot AEM-auteurs beheren met Adobe IMS via de Adobe Admin Console.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        Verifieer de gebruiker van uw website aan IDP gebruikend de integratie van SAML 2.0 van de Dienst van de Publicatie AEM.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="Token" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">Tokenverificatie</a></strong></div>
      <p>
        Toepassingen en middleware kunnen worden geverifieerd voor AEM met behulp van een API-servicetoken.
      </p>
    </td>   
  </tr>
</table>