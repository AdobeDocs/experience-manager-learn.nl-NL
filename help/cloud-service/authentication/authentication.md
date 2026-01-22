---
title: Verificatie in AEM as a Cloud Service
description: Meer informatie over verificatie in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Developer
level: Intermediate
jira: KT-10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
duration: 28
source-git-commit: 447ce616f043a67cb493507fc3de593859a53cb9
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# AEM as a Cloud Service-verificatie

AEM as a Cloud Service ondersteunt meerdere verificatieopties en varieert per servicetype.

|                       | AEM-auteur | AEM Publiceren |
|-----------------------|:----------:|:-----------:|
| [ IMS van Adobe ](../accessing/overview.md)<br>*(de Voorproef van AEM steunt Adobe IMS niet)* | ✔ | ✔ |
| [ OpenID verbindt (OIDC) ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/open-id-connect-support-for-aem-as-a-cloud-service-on-publish-tier) | ✘ | ✔ |
| [ SAML 2.0 via Adobe IMS ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html#how-to-set-up) | ✔ | ✔ |
| [ SAML 2.0 ](./saml-2-0.md) | ✘ | ✔ |
| [ enig-teken (SSO) ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [ OAuth ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [ Symbolische authentificatie ](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |
| Basisverificatie | ✘ | ✘ |

## Verificatieopties

Klik in de overeenkomstige verbinding hieronder aan voor details over opstelling en gebruik de authentificatiebenadering.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md"> IMS van Adobe </a></strong></div>
      <p>
          Toegang tot AEM-auteurs beheren met Adobe IMS via de Adobe Admin Console.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md"> SAML 2.0 </a></strong></div>
      <p>
        Verifieer de gebruiker van uw website aan IDP gebruikend de integratie van SAML 2.0 van de Dienst van de Publicatie van AEM.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="Token" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md"> Symbolische authentificatie </a></strong></div>
      <p>
        Toepassingen en middleware kunnen met een API-servicetoken worden geverifieerd op AEM.
      </p>
    </td>   
  </tr>
</table>
