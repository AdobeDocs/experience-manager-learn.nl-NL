---
title: Verificatieondersteuning in AEM 6.x
description: Een geconsolideerde weergave in de verificatiemechanismen die worden ondersteund door AEM 6.x.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
doc-type: Article
jira: KT-406
topic: Architecture
role: Architect
level: Experienced
exl-id: 96c542ae-6ab6-4d8a-94df-a58b03469320
last-substantial-update: 2022-09-10T00:00:00Z
thumbnail: KT-406.jpg
duration: 22
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 0%

---

# Verificatieondersteuning in AEM 6.x

Een geconsolideerde weergave van de door AEM ondersteunde authenticatiemechanismen (en soms ook autorisatiemechanismen).

*de volgende lijst beschrijft hoe de gebruikers in AEM voor authentiek kunnen verklaren.*

<table>
    <tbody>
        <tr>
            <td><strong>Verificatie</strong></td>
            <td><strong>AEM 6.3</strong></td>
            <td><strong>AEM 6.4</strong></td>
            <td><strong>AEM 6.5</strong></td>
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
            <td>Op token-gebaseerd (met <a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/encapsulated-token.html?lang=nl-NL" target="_blank"> ingekapseld teken </a>)</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Niet-AEM-systeem als canonieke identiteitsprovider</strong></td>
            <td></td>
            <td></td>
            <td></td>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/ldap-config.html?lang=nl-NL" target="_blank">LDAP</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/single-sign-on.html?lang=nl-NL" target="_blank">SSO</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html?lang=nl-NL" target="_blank">SAML 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/events/assets/oauth-server-functionality-in-aem-7-23-14.pdf?lang=nl-NL" target="_blank">OAuth 1.0a &amp; 2.0</a></td>
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

⁕ *die via communautaire projecten wordt verstrekt, maar niet direct door Adobe wordt gesteund.*
