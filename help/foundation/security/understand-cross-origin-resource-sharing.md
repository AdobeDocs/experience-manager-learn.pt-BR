---
title: Entenda o compartilhamento de recursos entre origens (CORS) com o AEM
description: O compartilhamento de recursos entre origens (CORS) do Adobe Experience Manager ajuda propriedades da web que não são do AEM a fazerem chamadas do lado do cliente para o AEM, tanto autenticadas quanto não autenticadas, a fim de obter conteúdo ou interagir diretamente com o AEM.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: Security, APIs
doc-type: Article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
duration: 240
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1011'
ht-degree: 100%

---

# Entenda o compartilhamento de recursos entre origens ([!DNL CORS])

O compartilhamento de recursos entre origens ([!DNL CORS]) do Adobe Experience Manager ajuda propriedades da web que não são do AEM a fazerem chamadas do lado do cliente para o AEM, tanto autenticadas quanto não autenticadas, para obter conteúdo ou interagir diretamente com o AEM.

A configuração de OSGI descrita neste documento é suficiente para:

1. Compartilhamento de recursos de origem única no AEM Publish
2. Acesso do CORS ao AEM Author

Se o acesso do CORS de várias origens for necessário no AEM Publish, consulte [esta documentação](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors#dispatcher-configuration).

## Configuração de OSGi da política de compartilhamento de recursos entre origens do Adobe Granite

As configurações do CORS são gerenciadas como fábricas de configuração de OSGi no AEM, sendo que cada política é representada como uma instância da fábrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuração de OSGi da política de compartilhamento de recursos entre origens do Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Seleção de política

Uma política é selecionada, comparando-se o

* `Allowed Origin` com o cabeçalho da solicitação `Origin`
* e `Allowed Paths` com o caminho da solicitação.

A primeira política correspondente a esses valores é usada. Se nenhuma for encontrada, qualquer solicitação [!DNL CORS] será negada.

Se nenhuma política for configurada, as solicitações [!DNL CORS] também não serão respondidas, pois o tratador estará desabilitado e, portanto, será efetivamente negado, desde que nenhum outro módulo do servidor responda a [!DNL CORS].

### Propriedades da política

#### [!UICONTROL Origens permitidas]

* `"alloworigin" <origin> | *`
* Lista de parâmetros `origin`, especificando URIs que podem acessar o recurso. Para solicitações sem credenciais, o servidor pode especificar &#42; como curinga, permitindo que qualquer origem acesse o recurso. *Não é recomendável usar `Allow-Origin: *` na produção, pois isso permite que qualquer site externo (ou seja, invasor) faça solicitações que, sem o CORS, são estritamente proibidas pelos navegadores.*

#### [!UICONTROL Origens permitidas (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lista de expressões regulares `regexp`, especificando URIs que podem acessar o recurso. *As expressões regulares podem levar a correspondências não intencionais, se não forem criadas com cuidado, permitindo que um invasor use um nome de domínio personalizado que também corresponda à política.* Em geral, é recomendável contar com políticas separadas para cada nome de host de origem específico, usando-se `alloworigin`, mesmo que isso signifique a configuração repetida das outras propriedades da política. Diferentes origens tendem a ter diferentes ciclos de vida e requisitos, beneficiando-se, assim, de uma separação clara.

#### [!UICONTROL Caminhos permitidos]

* `"allowedpaths" <regexp>`
* Lista de expressões regulares `regexp`, especificando caminhos de recursos aos quais a política se aplica.

#### [!UICONTROL Cabeçalhos expostos]

* `"exposedheaders" <header>`
* Lista de parâmetros de cabeçalho que indica os cabeçalhos de resposta que os navegadores têm permissão para acessar. Para solicitações de CORS (não simuladas), se não estiverem em branco, estes valores serão copiados no cabeçalho de resposta `Access-Control-Expose-Headers`. Então, os valores na lista (nomes de cabeçalho) ficam acessíveis ao navegador. Sem eles, esses cabeçalhos não são legíveis pelo navegador.

#### [!UICONTROL Idade máxima]

* `"maxage" <seconds>`
* Um parâmetro `seconds` que indica por quanto tempo os resultados de uma solicitação simulada podem ser armazenados em cache.

#### [!UICONTROL Cabeçalhos aceitos]

* `"supportedheaders" <header>`
* Lista de parâmetros `header`, indicando quais cabeçalhos de solicitação HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Métodos permitidos]

* `"supportedmethods"`
* Lista de parâmetros de método que indica quais métodos HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Permite credenciais]

* `"supportscredentials" <boolean>`
* Um `boolean`, indicando se a resposta à solicitação pode ou não ser exposta ao navegador. Quando usado como parte de uma resposta a uma solicitação simulada, indica se a solicitação real pode ou não ser feita por meio de credenciais.

### Exemplos de configurações

O site 1 é um caso básico, acessível anonimamente e somente de leitura, no qual o conteúdo é consumido por meio de solicitações [!DNL GET]:

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
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
    "Access-Control-Request-Headers"
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

## Receios e configuração do armazenamento em cache do Dispatcher {#dispatcher-caching-concerns-and-configuration}

A partir do Dispatcher 4.1.1+, os cabeçalhos de resposta podem ser armazenados em cache. Isso permite o armazenamento em cache de cabeçalhos [!DNL CORS] junto com os recursos solicitados por [!DNL CORS], desde que a solicitação seja anônima.

Geralmente, as mesmas considerações para armazenamento em cache do conteúdo no Dispatcher podem ser aplicadas ao armazenamento em cache de cabeçalhos de resposta do CORS no Dispatcher. A tabela a seguir define quando cabeçalhos de [!DNL CORS] (e, portanto, solicitações [!DNL CORS]) podem ser armazenados em cache.

| Armazenável em cache | Ambiente | Status de autenticação | Explicação |
|-----------|-------------|-----------------------|-------------|
| Não | AEM Publish | Autenticado | O armazenamento em cache no Dispatcher do AEM Author é limitado a ativos estáticos e não criados. Isso dificulta e impossibilita o armazenamento em cache da maioria dos recursos no AEM Author, incluindo cabeçalhos de resposta HTTP. |
| Não | AEM Publish | Autenticado | Evite armazenar em cache cabeçalhos do CORS em solicitações autenticadas. Isso está de acordo com a orientação comum de não armazenar solicitações autenticadas em cache, pois é difícil determinar como o status de autenticação/autorização do usuário solicitante afetará o recurso entregue. |
| Sim | AEM Publish | Anônimo | As solicitações anônimas que podem ser armazenadas em cache no Dispatcher também podem ter seus cabeçalhos de resposta em cache, garantindo que solicitações futuras do CORS possam acessar o conteúdo em cache. Qualquer alteração na configuração do CORS no AEM Publish **precisa** ser seguida por uma invalidação dos recursos em cache afetados. As práticas recomendadas determinam as implantações de código ou de configuração nas quais o cache do Dispatcher é removido, já que é difícil determinar qual conteúdo em cache pode ser afetado. |

### Permitir cabeçalhos de solicitação do CORS

Para permitir que os [cabeçalhos de solicitação HTTP necessários sejam transmitidos para o AEM para processamento](https://experienceleague.adobe.com/pt-br/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration#specifying-the-http-headers-to-pass-through-clientheaders), eles precisam ser permitidos na configuração `/clientheaders` do Dispatcher.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Armazenamento em cache de cabeçalhos de resposta do CORS

Para permitir o armazenamento em cache e a veiculação de cabeçalhos do CORS no conteúdo em cache, adicione a [configuração /cache /headers](https://experienceleague.adobe.com/pt-br/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration#caching-http-response-headers) ao arquivo `dispatcher.any` do AEM Publish.

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

Lembre-se de **reiniciar o aplicativo do servidor da web** depois de fazer alterações no arquivo `dispatcher.any`.

Provavelmente, será necessário limpar todo o cache para garantir que os cabeçalhos sejam armazenados em cache de maneira adequada na próxima solicitação após uma atualização da configuração `/cache/headers`.

## Resolução de problemas do CORS

O registro em log está disponível no `com.adobe.granite.cors`:

* habilite `DEBUG` para ver mais detalhes sobre o motivo de uma solicitação [!DNL CORS] ter sido negada
* habilite `TRACE` para ver mais detalhes sobre todas as solicitações que passam pelo tratador de CORS

### Dicas:

* Recrie solicitações XHR com curl manualmente, mas certifique-se de copiar todos os cabeçalhos e detalhes, pois todos eles podem fazer a diferença. Alguns consoles de navegador permitem copiar o comando curl
* Verifique se a solicitação foi negada pelo tratador de CORS, não pela autenticação, filtro de token CSRF, filtros do Dispatcher ou outras camadas de segurança
   * Se o tratador de CORS responder com 200, mas o cabeçalho `Access-Control-Allow-Origin` estiver ausente na resposta, procure negações nos logs em [!DNL DEBUG], em `com.adobe.granite.cors`
* Se o cache do Dispatcher de solicitações [!DNL CORS] estiver habilitado
   * Verifique se a configuração `/cache/headers` foi aplicada a `dispatcher.any` e se o servidor da web foi reiniciado com sucesso
   * Verifique se o cache foi limpo corretamente após qualquer alteração na configuração de OSGi ou do Dispatcher.
* se necessário, verifique a presença de credenciais de autenticação na solicitação.

## Materiais de apoio

* [Fábrica de configuração de OSGi do AEM para políticas de compartilhamento de recursos entre origens](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Compartilhamento de recursos entre origens (W3C)](https://www.w3.org/TR/cors/)
* [Controle de acesso HTTP (Mozilla MDN)](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Guides/CORS)
