---
title: Verificatieondersteuning begrijpen in AEM
description: 'Een geconsolideerde weergave van de door AEM ondersteunde verificatiemechanismen (en soms autorisaties). '
version: 6.3, 6.4, 6.5
feature: 'Gebruikers en groepen '
topics: authentication, security
activity: understand
audience: architect, developer, implementer
doc-type: article
kt: 406
topic: Architectuur
role: Architect
level: Ervaren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 2%

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
            <td>Op token gebaseerd (met <a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">ingekapseld token</a>)</td>
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
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/ldap-config.html" target="_blank">LDAP</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/deploying/configuring/single-sign-on.html" target="_blank">SSO</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html" target="_blank">SAML 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-oauth-server-functionality-in-aem.html" target="_blank">OAuth 1.0a &amp; 2.0</a></td>
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

⁕ *Wordt geleverd via gemeenschapsprojecten, maar niet rechtstreeks ondersteund door Adobe.*
