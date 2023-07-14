---
title: Proteção CSRF
description: Saiba como gerar e adicionar tokens CSRF AEM a POST, PUT AEM e solicitações de exclusão permitidas do para usuários autenticados.
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
source-git-commit: 9cd2c5b6337ae7a4b05380d6a3fb945f28a14299
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---


# Proteção CSRF

Saiba como gerar e adicionar tokens CSRF AEM a POST, PUT AEM e solicitações de exclusão permitidas do para usuários autenticados.

O AEM requer que um token CSRF válido seja enviado para __autenticado__ __POST__, __PUT ou __DELETE__ Solicitações HTTP para os serviços de Autor e Publicação do AEM.

O token CSRF não é necessário para __GET__ solicitações ou __anônimo__ solicitações.

Se um token CSRF não for enviado com uma solicitação POST, PUT ou DELETE, o AEM AEM retornará uma resposta 403 Proibido e o registrará o seguinte erro:

```log
[INFO][POST path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

Consulte a [documentação para obter mais detalhes sobre a proteção AEM CSRF](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html).


## Biblioteca cliente CSRF

O AEM fornece uma biblioteca de cliente que pode ser usada para gerar e adicionar tokens CSRF XHR e solicitações de POST de formulário, por meio do patch de funções de protótipo principais. A funcionalidade é fornecida pela variável `granite.csrf.standalone` categoria da biblioteca do cliente.

Para usar essa abordagem, adicione `granite.csrf.standalone` como uma dependência ao carregamento da biblioteca do cliente na sua página. Por exemplo, se estiver usando a variável `wknd.site` categoria da biblioteca do cliente, adicionar `granite.csrf.standalone` como uma dependência ao carregamento da biblioteca do cliente na sua página.

## Envio de formulário personalizado com proteção CSRF

Se a utilização de [`granite.csrf.standalone` biblioteca do cliente](#csrf-client-library) não for compatível com seu caso de uso, você poderá adicionar manualmente um token CSRF ao envio de um formulário. O exemplo a seguir mostra como adicionar um token CSRF ao envio de um formulário.

Este trecho de código demonstra como, durante o envio de um formulário, o token CSRF pode ser obtido do AEM e adicionado a uma entrada de formulário chamada `:cq_csrf_token`. Como o token CSRF tem uma vida útil curta, é melhor recuperar e definir o token CSRF imediatamente antes do envio do formulário, garantindo sua validade.

```javascript
// Attach submit handler event to form onSubmit
document.querySelector('form').addEventListener('submit', (e) => {
    e.preventDefault();

    const form = e.target;
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    
    // Create a form input named ``:cq_csrf_token`` with the CSRF token.
    let csrfTokenInput = form.querySelector('input[name=":cq_csrf_token"]');
    if (!csrfTokenInput?.value) {
        // If the form does not have a CSRF token input, add one.
        form.insertAdjacentHTML('afterbegin', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }

    form.submit();
});
```

## Buscar com proteção CSRF

Se a utilização de [`granite.csrf.standalone` biblioteca do cliente](#csrf-client-library) não for compatível com seu caso de uso, é possível adicionar manualmente um token CSRF a um XHR ou buscar solicitações. O exemplo a seguir mostra como adicionar um token CSRF a um XHR criado com a busca.

Este trecho de código demonstra como buscar um token CSRF do AEM e adicioná-lo a solicitações de busca `CSRF-Token` cabeçalho da solicitação HTTP. Como o token CSRF tem uma vida útil curta, é melhor recuperar e definir o token CSRF imediatamente antes de fazer a solicitação de busca, garantindo sua validade.

```javascript
/**
 * Get CSRF token from AEM.
 * CSRF token expire after a few minutes, so it is best to get the token before each request.
 * 
 * @returns {Promise<string>} that resolves to the CSRF token.
 */
async getCsrfToken() {
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    return json.token;
}
...
// Fetch from AEM with CSRF token in a header named 'CSRF-Token'.
await fetch('/path/to/aem/endpoint', {
    method: 'POST',
    headers: {
        'CSRF-Token': await getCsrfToken()
    },
});
```

## Configuração do Dispatcher

Ao usar tokens CSRF no serviço de publicação do AEM, a configuração do Dispatcher deve ser atualizada para permitir solicitações do GET para o endpoint do token CSRF. A configuração a seguir permite solicitações do GET para o endpoint do token CSRF no serviço de publicação do AEM. Se essa configuração não for adicionada, o endpoint do token CSRF retornará uma resposta 404 Não encontrado.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
