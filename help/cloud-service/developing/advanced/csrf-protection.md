---
title: Proteção CSRF
description: Saiba como gerar e adicionar tokens AEM CSRF a solicitações de POST, PUT e Exclusão permitidas no AEM para usuários autenticados.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
exl-id: 747322ed-f01a-48ba-a4a0-483b81f1e904
duration: 125
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---

# Proteção CSRF

Saiba como gerar e adicionar tokens AEM CSRF a solicitações de POST, PUT e Exclusão permitidas no AEM para usuários autenticados.

O AEM requer que um token CSRF válido seja enviado para solicitações HTTP __autenticadas__ __POST__, __PUT ou __DELETE__ para os serviços de Autor e Publicação do AEM.

O token CSRF não é necessário para solicitações __GET__ ou __anônimas__.

Se um token CSRF não for enviado com uma solicitação POST, PUT ou DELETE, o AEM retornará uma resposta 403 Proibido e a AEM registrará o seguinte erro:

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

Consulte a [documentação para obter mais detalhes sobre a proteção CSRF da AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=pt-BR).


## Biblioteca cliente CSRF

O AEM fornece uma biblioteca do cliente que pode ser usada para gerar e adicionar tokens CSRF XHR e solicitações de POST de formulário, por meio do patch de funções de protótipo principais. A funcionalidade é fornecida pela categoria de biblioteca cliente `granite.csrf.standalone`.

Para usar essa abordagem, adicione `granite.csrf.standalone` como uma dependência ao carregamento da biblioteca do cliente na sua página. Por exemplo, se você estiver usando a categoria de biblioteca de cliente `wknd.site`, adicione `granite.csrf.standalone` como uma dependência ao carregamento da biblioteca de cliente na sua página.

## Envio de formulário personalizado com proteção CSRF

Se o uso da biblioteca [`granite.csrf.standalone` do cliente ](#csrf-client-library) não for compatível com seu caso de uso, você poderá adicionar manualmente um token CSRF ao envio de um formulário. O exemplo a seguir mostra como adicionar um token CSRF ao envio de um formulário.

Este trecho de código demonstra como, quando um formulário é enviado, o token CSRF pode ser buscado no AEM e adicionado a uma entrada de formulário chamada `:cq_csrf_token`. Como o token CSRF tem uma vida útil curta, é melhor recuperar e definir o token CSRF imediatamente antes do envio do formulário, garantindo sua validade.

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

## Buscar com proteção CSRF

Se o uso da biblioteca [`granite.csrf.standalone` do cliente ](#csrf-client-library) não for compatível com seu caso de uso, você poderá adicionar manualmente um token CSRF a um XHR ou buscar solicitações. O exemplo a seguir mostra como adicionar um token CSRF a um XHR criado com a busca.

Este trecho de código demonstra como buscar um token CSRF do AEM e adicioná-lo ao cabeçalho de solicitação HTTP `CSRF-Token` de uma solicitação de busca. Como o token CSRF tem uma vida útil curta, é melhor recuperar e definir o token CSRF imediatamente antes de fazer a solicitação de busca, garantindo sua validade.

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

## Configuração do Dispatcher

Ao usar tokens CSRF no serviço de publicação do AEM, a configuração do Dispatcher deve ser atualizada para permitir solicitações do GET para o endpoint do token CSRF. A configuração a seguir permite que o GET faça solicitações para o endpoint do token CSRF no serviço de publicação do AEM. Se essa configuração não for adicionada, o endpoint do token CSRF retornará uma resposta 404 Não encontrado.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
