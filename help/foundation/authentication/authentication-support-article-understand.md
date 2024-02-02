---
title: Verificatieondersteuning in AEM 6.x
description: Een geconsolideerde mening in de authentificatiemechanismen die door AEM 6.x worden gesteund.
version: 6.4, 6.5
feature: User and Groups
doc-type: Article
jira: KT-406
topic: Architecture
role: Architect
level: Experienced
exl-id: 96c542ae-6ab6-4d8a-94df-a58b03469320
last-substantial-update: 2022-09-10T00:00:00Z
thumbnail: KT-406.jpg
duration: 38
source-git-commit: 9ef35d4d20ea574ac2051d7d3e997ecc18c2aaaa
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 0%

---

# Verificatieondersteuning in AEM 6.x

Een geconsolideerde weergave van de door AEM ondersteunde verificatiemechanismen (en soms autorisaties).

*In de volgende tabel wordt beschreven hoe gebruikers zich in AEM kunnen verifiëren.*

<table>
    <tbody>
        <tr>
            <td><strong>Verificatie</strong></td>
            <td><strong>AEM 6,3</strong></td>
            <td><strong>AEM 6,4</strong></td>
            <td><strong>AEM 6,5</strong></td>
        </tr>
        <tr>
            <td><strong>AEM als canonieke identiteitsprovider</strong></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>Basisverificatie</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Op Forms gebaseerd</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Op basis van token (w/ <a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">ingekapseld token</a>)</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Niet-AEM systeem als canonieke identiteitsprovider</strong></td>
            <td></td>
            <td></td>
            <td></td>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/ldap-config.html" target="_blank">LDAP</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/single-sign-on.html" target="_blank">SSO</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html" target="_blank">SAML 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/events/assets/oauth-server-functionality-in-aem-7-23-14.pdf" target="_blank">OAuth 1.0a &amp; 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://sling.apache.org/documentation/the-sling-engine/authentication/authentication-authenticationhandler/openid-authenticationhandler.html" target="_blank">OpenID</a></td>
                <td>⁕</td>
                <td>⁕</td>
                <td>*</td>
            </tr>
    </tbody>
</table>

⁕ *Via gemeenschapsprojecten verstrekt, maar niet rechtstreeks ondersteund door Adobe.*
