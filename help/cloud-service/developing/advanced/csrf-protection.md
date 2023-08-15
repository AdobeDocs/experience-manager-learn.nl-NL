---
title: CSRF-bescherming
description: Leer hoe u AEM CSRF-tokens kunt genereren en toevoegen aan toegestane POST-, PUT- en verwijderverzoeken om AEM voor geverifieerde gebruikers.
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
source-git-commit: 38db146129ceab83af50bf97cd6eb2d7179adbbf
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---


# CSRF-bescherming

Leer hoe u AEM CSRF-tokens kunt genereren en toevoegen aan toegestane POST-, PUT- en verwijderverzoeken om AEM voor geverifieerde gebruikers.

AEM vereist een geldig CSRF-token voor __geverifieerd__ __POST__, __PUT, of __DELETE__ HTTP-aanvragen bij zowel AEM Auteur- als publicatieservices.

De token CSRF is niet vereist voor __GET__ verzoeken, of __anoniem__ verzoeken.

Als een teken CSRF niet samen met een POST, een PUT, of een verzoek van DELETE wordt verzonden, AEM keert een 403 Verboden reactie terug, en AEM registreert de volgende fout:

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

Zie de [documentatie voor meer informatie over AEM CSRF-bescherming](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html).


## CSRF-clientbibliotheek

AEM biedt een clientbibliotheek die kan worden gebruikt om CSRF-tokens XHR- en form POST-aanvragen te genereren en toe te voegen, door kernprototypefuncties te patchen. De functionaliteit wordt geleverd door de `granite.csrf.standalone` clientbibliotheekcategorie.

Als u deze methode wilt gebruiken, voegt u `granite.csrf.standalone` als een afhankelijkheid van de clientbibliotheek die op uw pagina wordt geladen. Als u bijvoorbeeld de opdracht `wknd.site` clientbibliotheekcategorie, toevoegen `granite.csrf.standalone` als een afhankelijkheid van de clientbibliotheek die op uw pagina wordt geladen.

## Aangepast formulier verzenden met CSRF-beveiliging

Als het gebruik van [`granite.csrf.standalone` clientbibliotheek](#csrf-client-library) Kan niet worden gebruikt, u kunt handmatig een CSRF-token toevoegen aan een formulier dat wordt verzonden. In het volgende voorbeeld ziet u hoe u een CSRF-token toevoegt aan een formulierverzending.

Dit codefragment demonstreert hoe bij het verzenden van formulieren de CSRF-token kan worden opgehaald van AEM en toegevoegd aan een formulierinvoer met de naam `:cq_csrf_token`. Omdat de token CSRF een korte levensduur heeft, is het beter om de token CSRF op te halen en in te stellen direct voordat het formulier wordt verzonden, zodat de geldigheid van de token gegarandeerd is.

```javascript
// Attach submit handler event to form onSubmit
document.querySelector('form').addEventListener('submit', async (event) => {
    event.preventDefault();

    const form = event.target;
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    
    // Create a form input named ``:cq_csrf_token`` with the CSRF token.
    let csrfTokenInput = form.querySelector('input[name=":cq_csrf_token"]');
    if (!csrfTokenInput?.value) {
        // If the form does not have a CSRF token input, add one.
        form.insertAdjacentHTML('beforeend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## Ophalen met KVP-beveiliging

Als het gebruik van [`granite.csrf.standalone` clientbibliotheek](#csrf-client-library) is niet geschikt voor uw gebruiksgeval, kunt u een symbolisch CSRF aan een XHR manueel toevoegen of verzoeken halen. In het volgende voorbeeld ziet u hoe u een CSRF-token toevoegt aan een XHR die is gemaakt met ophalen.

Dit codefragment toont aan hoe te om een symbolisch CSRF van AEM te halen, en het toe te voegen aan een haalverzoek `CSRF-Token` HTTP request header. Omdat het teken CSRF een kort leven heeft, is het best om het teken terug te winnen en te plaatsen CSRF onmiddellijk alvorens het haalverzoek wordt gemaakt, die zijn geldigheid verzekeren.

```javascript
/**
 * Get CSRF token from AEM.
 * CSRF token expire after a few minutes, so it is best to get the token before each request.
 * 
 * @returns {Promise<string>} that resolves to the CSRF token.
 */
async function getCsrfToken() {
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    return json.token;
}

// Fetch from AEM with CSRF token in a header named 'CSRF-Token'.
await fetch('/path/to/aem/endpoint', {
    method: 'POST',
    headers: {
        'CSRF-Token': await getCsrfToken()
    },
});
```

## Dispatcher-configuratie

Wanneer het gebruiken van tokens CSRF op AEM publicatiedienst, moet de configuratie van de Verzender worden bijgewerkt om GET- verzoeken aan het symbolische eindpunt toe te staan CSRF. De volgende configuratie staat GET verzoeken aan het symbolische eindpunt CSRF op de AEM toe publiceren dienst. Als deze configuratie niet wordt toegevoegd, keert het symbolische eindpunt CSRF een 404 niet Gevonden reactie terug.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
