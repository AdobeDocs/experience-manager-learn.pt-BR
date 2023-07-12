---
title: Entenda o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens) com AEM
description: O Compartilhamento de recursos entre origens (CORS) da Adobe Experience Manager facilita que propriedades da Web que não sejam de AEM façam chamadas do lado do cliente para o AEM, tanto autenticadas quanto não autenticadas, para buscar conteúdo ou interagir diretamente com o AEM.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
topics: security, development, content-delivery
feature: Security, APIs
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: 325c0204c33686e09deb82dd159557e0b8743df6
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 2%

---

# Entender o compartilhamento de recursos entre origens ([!DNL CORS])

Compartilhamento de recursos entre origens da Adobe Experience Manager ([!DNL CORS]) facilita as propriedades da Web que não são do AEM para fazer chamadas do lado do cliente para o AEM, tanto autenticadas quanto não autenticadas, para buscar conteúdo ou interagir diretamente com o AEM.

## Configuração OSGi da Política de Compartilhamento de Recursos entre Origens do Adobe Granite

As configurações do CORS são gerenciadas como fábricas de configuração OSGi no AEM, com cada política sendo representada como uma instância da fábrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuração OSGi da Política de Compartilhamento de Recursos entre Origens do Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Seleção de política

Uma política é selecionada comparando-se as

* `Allowed Origin` com o `Origin` cabeçalho da solicitação
* e `Allowed Paths` com o caminho da solicitação.

A primeira política correspondente a esses valores é usada. Se nenhum for encontrado, qualquer [!DNL CORS] solicitação negada.

Se nenhuma política estiver configurada, [!DNL CORS] as solicitações também não serão respondidas, pois o manipulador está desativado e, portanto, efetivamente negado - desde que nenhum outro módulo do servidor responda a [!DNL CORS].

### Propriedades da política

#### [!UICONTROL Origens permitidas]

* `"alloworigin" <origin> | *`
* Lista de `origin` parâmetros especificando URIs que podem acessar o recurso. Para solicitações sem credenciais, o servidor pode especificar &#42; como curinga, permitindo que qualquer origem acesse o recurso. *Não é recomendável usar `Allow-Origin: *` em produção, pois permite que cada site estrangeiro (ou seja, invasor) faça solicitações que sem o CORS sejam estritamente proibidas pelos navegadores.*

#### [!UICONTROL Origens permitidas (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lista de `regexp` expressões regulares que especificam URIs que podem acessar o recurso. *Expressões regulares podem levar a correspondências não intencionais se não forem criadas com cuidado, permitindo que um invasor use um nome de domínio personalizado que também corresponda à política.* Geralmente, é recomendável ter políticas separadas para cada nome de host de origem específico, usando `alloworigin`, mesmo que isso signifique a configuração repetida das outras propriedades de política. Diferentes origens tendem a ter diferentes ciclos de vida e requisitos, beneficiando assim de uma separação clara.

#### [!UICONTROL Caminhos permitidos]

* `"allowedpaths" <regexp>`
* Lista de `regexp` expressões regulares que especificam caminhos de recursos aos quais a política se aplica.

#### [!UICONTROL Cabeçalhos expostos]

* `"exposedheaders" <header>`
* Lista de parâmetros de cabeçalho que indica os cabeçalhos de resposta que os navegadores têm permissão para acessar.

#### [!UICONTROL Idade máxima]

* `"maxage" <seconds>`
* A `seconds` parâmetro que indica por quanto tempo os resultados de uma solicitação antes do voo podem ser armazenados em cache.

#### [!UICONTROL Cabeçalhos suportados]

* `"supportedheaders" <header>`
* Lista de `header` parâmetros que indicam quais cabeçalhos HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Métodos permitidos]

* `"supportedmethods"`
* Lista de parâmetros de método que indica quais métodos HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Suporta Credenciais]

* `"supportscredentials" <boolean>`
* A `boolean` indicando se a resposta à solicitação pode ou não ser exposta ao navegador. Quando usado como parte de uma resposta a uma solicitação antes do voo, indica se a solicitação real pode ou não ser feita usando credenciais.

### Exemplo de configurações

O Site 1 é um cenário básico, acessível anonimamente e somente leitura, em que o conteúdo é consumido via [!DNL GET] solicitações:

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "OPTIONS"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site1.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site1\.com"
  ],
  "allowedpaths":[
    "/content/_cq_graphql/site1/endpoint.json",
    "/graphql/execute.json.*",
    "/content/site1/.*"
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
  ]
}
```

O site 2 é mais complexo e requer solicitações autorizadas e mutantes (POST, PUT, DELETE):

```json
{
  "supportscredentials":true,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
    "POST",
    "DELETE",
    "OPTIONS",
    "PUT"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site2.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site2\.com"
  ],
  "allowedpaths":[
    "/content/site2/.*",
    "/libs/granite/csrf/token.json",
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization",
    "CSRF-Token"
  ]
}
```

## Preocupações e configuração do armazenamento em cache do Dispatcher {#dispatcher-caching-concerns-and-configuration}

A partir do Dispatcher 4.1.1+, os cabeçalhos de resposta podem ser armazenados em cache. Isso permite armazenar em cache [!DNL CORS] cabeçalhos ao longo do [!DNL CORS]-recursos solicitados, desde que a solicitação seja anônima.

Geralmente, as mesmas considerações para armazenamento em cache de conteúdo no Dispatcher podem ser aplicadas ao armazenamento em cache de cabeçalhos de resposta do CORS no Dispatcher. A tabela a seguir define quando [!DNL CORS] cabeçalhos (e assim [!DNL CORS] solicitações) podem ser armazenadas em cache.

| Armazenável em cache | Ambiente | Status de autenticação | Explicação |
|-----------|-------------|-----------------------|-------------|
| Não | AEM Publish | Autenticado | O armazenamento em cache do Dispatcher no AEM Author é limitado a ativos estáticos, não criados. Isso dificulta e impossibilita o armazenamento em cache da maioria dos recursos no AEM Author, incluindo cabeçalhos de resposta HTTP. |
| Não | AEM Publish | Autenticado | Evite armazenar cabeçalhos CORS em cache em solicitações autenticadas. Isso se alinha à orientação comum de não armazenar solicitações autenticadas em cache, pois é difícil determinar como o status de autenticação/autorização do usuário solicitante afetará o recurso entregue. |
| Sim | AEM Publish | Anônimo | As solicitações anônimas que podem ser armazenadas em cache no dispatcher também podem ter seus cabeçalhos de resposta em cache, garantindo que as futuras solicitações do CORS possam acessar o conteúdo em cache. Qualquer alteração na configuração do CORS na publicação do AEM **deve** ser seguido por uma invalidação dos recursos em cache afetados. As práticas recomendadas determinam as implantações de código ou configuração nas quais o cache do dispatcher é removido, já que é difícil determinar qual conteúdo em cache pode ser afetado. |

### Permitir cabeçalhos de solicitação CORS

Para permitir as [Cabeçalhos de solicitação HTTP para transmitir ao AEM para processamento](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders), eles devem ter permissão no do Dispatcher `/clientheaders` configuração.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Armazenamento em cache de cabeçalhos de resposta do CORS

Para permitir o armazenamento em cache e a veiculação de cabeçalhos CORS no conteúdo em cache, adicione o seguinte [/cache /configuração de cabeçalhos](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=pt-BR#caching-http-response-headers) para o AEM Publish `dispatcher.any` arquivo.

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

Lembre-se de **reiniciar o aplicativo do servidor web** depois de fazer alterações no `dispatcher.any` arquivo.

Provavelmente, é necessário limpar totalmente o cache para garantir que os cabeçalhos sejam armazenados em cache corretamente na próxima solicitação após um `/cache/headers` atualização de configuração.

## Solução de problemas do CORS

O registro está disponível em `com.adobe.granite.cors`:

* habilitar `DEBUG` para ver detalhes sobre por que uma [!DNL CORS] a solicitação foi negada
* habilitar `TRACE` para ver detalhes sobre todas as solicitações que passam pelo manipulador CORS

### Dicas:

* Recriar manualmente solicitações XHR usando curl, mas certifique-se de copiar todos os cabeçalhos e detalhes, pois cada um pode fazer a diferença; alguns consoles do navegador permitem copiar o comando curl
* Verifique se a solicitação foi negada pelo manipulador CORS e não pela autenticação, filtro de token CSRF, filtros de dispatcher ou outras camadas de segurança
   * Se o manipulador CORS responder com 200, mas `Access-Control-Allow-Origin` cabeçalho estiver ausente na resposta, revise os logs para negações em [!DNL DEBUG] in `com.adobe.granite.cors`
* Se o armazenamento em cache do dispatcher de [!DNL CORS] solicitações está habilitado
   * Assegure a `/cache/headers` a configuração é aplicada a `dispatcher.any` e o servidor Web foi reiniciado com êxito
   * Verifique se o cache foi limpo corretamente após qualquer alteração de configuração de OSGi ou dispatcher.
* se necessário, verifique a presença de credenciais de autenticação na solicitação.

## Materiais de suporte

* [Fábrica de configuração OSGi do AEM para políticas de compartilhamento de recursos entre origens](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Compartilhamento de recursos entre origens (W3C)](https://www.w3.org/TR/cors/)
* [Controle de acesso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
