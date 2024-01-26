---
title: Verificatie in AEM as a Cloud Service
description: Leer over authentificatie in AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
duration: 51
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 0%

---

# as a Cloud Service verificatie AEM

AEM as a Cloud Service steunt veelvoudige van authentificatieopties en varieert door de diensttype.

|                       | AEM auteur | AEM publiceren |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| ・ [SAML 2.0 via Adobe IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html#how-to-set-up) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [Single Sign On (SSO)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [OAuth](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [Tokenverificatie](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |

## Verificatieopties

Klik in de overeenkomstige verbinding hieronder aan voor details over opstelling en gebruik de authentificatiebenadering.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Toegang tot AEM auteur beheren met Adobe IMS via de Adobe Admin Console.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        Verifieer de gebruiker van uw website aan IDP gebruikend AEM de integratie van SAML 2.0 van de Publish dienst.
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
