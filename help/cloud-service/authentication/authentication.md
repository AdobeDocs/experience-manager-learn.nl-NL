---
title: Verificatie in AEM as a Cloud Service
description: Leer over authentificatie in AEM as a Cloud Service's.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
source-git-commit: ad9aa172d37741207dabcbc705efaa851fd17e7c
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 0%

---

# as a Cloud Service verificatie AEM

AEM as a Cloud Service steunt veelvoudige van authentificatieopties en varieert door de diensttype.

|  | AEM-auteur | AEM-publicatie |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| ・ [SAML 2.0 via Adobe IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html#how-to-set-up) | ✔ | ✘ |
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
