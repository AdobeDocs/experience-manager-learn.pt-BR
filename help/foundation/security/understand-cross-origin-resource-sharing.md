---
title: Entenda o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens) com AEM
description: O Compartilhamento de recursos entre origens (CORS) da Adobe Experience Manager facilita que propriedades da Web que não sejam de AEM façam chamadas do lado do cliente para o AEM, tanto autenticadas quanto não autenticadas, para buscar conteúdo ou interagir diretamente com o AEM.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: Security, APIs
doc-type: Article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
duration: 240
source-git-commit: 6922d885c25d0864560ab3b8e38907060ff3cc70
workflow-type: tm+mt
source-wordcount: '1011'
ht-degree: 1%

---

# Entender o Compartilhamento de Recursos entre Origens ([!DNL CORS])

O Compartilhamento de Recursos entre Origens ([!DNL CORS]) da Adobe Experience Manager facilita que propriedades da Web que não sejam de AEM façam chamadas do lado do cliente para AEM, tanto autenticadas quanto não autenticadas, para buscar conteúdo ou interagir diretamente com o AEM.

A configuração OSGI descrita neste documento é suficiente para:

1. Compartilhamento de recursos de origem única no AEM Publish
2. Acesso do CORS ao AEM Author

Se o acesso ao CORS de várias origens for necessário no AEM Publish, consulte [esta documentação](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration).

## Configuração OSGi da Política de Compartilhamento de Recursos entre Origens do Adobe Granite

As configurações do CORS são gerenciadas como fábricas de configuração OSGi no AEM, com cada política sendo representada como uma instância da fábrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuração OSGi da Política de Compartilhamento de Recursos entre Origens do Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Seleção de política

Uma política é selecionada comparando-se as

* `Allowed Origin` com o cabeçalho da solicitação `Origin`
* e `Allowed Paths` com o caminho da solicitação.

A primeira política correspondente a esses valores é usada. Se nenhum for encontrado, qualquer solicitação [!DNL CORS] será negada.

Se nenhuma política estiver configurada, as solicitações [!DNL CORS] também não serão respondidas, pois o manipulador está desabilitado e, portanto, efetivamente negado - desde que nenhum outro módulo do servidor responda a [!DNL CORS].

### Propriedades da política

#### [!UICONTROL Origens permitidas]

* `"alloworigin" <origin> | *`
* Lista de parâmetros `origin` especificando URIs que podem acessar o recurso. Para solicitações sem credenciais, o servidor pode especificar &#42; como curinga, permitindo que qualquer origem acesse o recurso. *Não é recomendável usar `Allow-Origin: *` na produção, pois ele permite que qualquer site externo (ou seja, invasor) faça solicitações que sem o CORS sejam estritamente proibidas pelos navegadores.*

#### [!UICONTROL Origens Permitidas (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lista de expressões regulares `regexp` especificando URIs que podem acessar o recurso. *As expressões regulares podem levar a correspondências não intencionais, se não forem criadas com cuidado, permitindo que um invasor use um nome de domínio personalizado que também corresponda à política.* Geralmente é recomendável ter políticas separadas para cada nome de host de origem específico, usando `alloworigin`, mesmo que isso signifique a configuração repetida das outras propriedades de política. Diferentes origens tendem a ter diferentes ciclos de vida e requisitos, beneficiando assim de uma separação clara.

#### [!UICONTROL Caminhos permitidos]

* `"allowedpaths" <regexp>`
* Lista de expressões regulares `regexp` especificando caminhos de recursos aos quais a política se aplica.

#### [!UICONTROL Cabeçalhos expostos]

* `"exposedheaders" <header>`
* Lista de parâmetros de cabeçalho que indica os cabeçalhos de resposta que os navegadores têm permissão para acessar. Para solicitações CORS (não antes da simulação), se não estiverem vazios, esses valores serão copiados no cabeçalho de resposta `Access-Control-Expose-Headers`. Os valores na lista (nomes de cabeçalho) são então acessíveis ao navegador; sem ela, esses cabeçalhos não são legíveis pelo navegador.

#### [!UICONTROL Idade Máxima]

* `"maxage" <seconds>`
* Um parâmetro `seconds` que indica por quanto tempo os resultados de uma solicitação de pré-impressão podem ser armazenados em cache.

#### [!UICONTROL Cabeçalhos com suporte]

* `"supportedheaders" <header>`
* Lista de parâmetros `header` indicando quais cabeçalhos de solicitação HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Métodos permitidos]

* `"supportedmethods"`
* Lista de parâmetros de método que indica quais métodos HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Dá Suporte A Credenciais]

* `"supportscredentials" <boolean>`
* Um `boolean` indicando se a resposta à solicitação pode ou não ser exposta ao navegador. Quando usado como parte de uma resposta a uma solicitação antes do voo, indica se a solicitação real pode ou não ser feita usando credenciais.

### Exemplo de configurações

O Site 1 é um cenário básico, acessível anonimamente, somente leitura, em que o conteúdo é consumido por meio de [!DNL GET] solicitações:

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

## Preocupações e configuração do armazenamento em cache do Dispatcher {#dispatcher-caching-concerns-and-configuration}

A partir do Dispatcher 4.1.1+, os cabeçalhos de resposta podem ser armazenados em cache. Isso permite o armazenamento em cache de cabeçalhos [!DNL CORS] junto com os recursos solicitados [!DNL CORS], desde que a solicitação seja anônima.

Geralmente, as mesmas considerações para armazenamento em cache de conteúdo no Dispatcher podem ser aplicadas ao armazenamento em cache de cabeçalhos de resposta do CORS no dispatcher. A tabela a seguir define quando [!DNL CORS] cabeçalhos (e, portanto, [!DNL CORS] solicitações) podem ser armazenados em cache.

| Armazenável em cache | Ambiente | Status de autenticação | Explicação |
|-----------|-------------|-----------------------|-------------|
| Não | Publicação no AEM | Autenticado | O armazenamento em cache do Dispatcher no AEM Author é limitado a ativos estáticos, não criados. Isso torna difícil e impraticável armazenar em cache a maioria dos recursos no AEM Author, incluindo cabeçalhos de resposta HTTP. |
| Não | Publicação no AEM | Autenticado | Evite armazenar cabeçalhos CORS em cache em solicitações autenticadas. Isso se alinha à orientação comum de não armazenar solicitações autenticadas em cache, pois é difícil determinar como o status de autenticação/autorização do usuário solicitante afetará o recurso entregue. |
| Sim | Publicação no AEM | Anônimo | As solicitações anônimas que podem ser armazenadas em cache no dispatcher também podem ter seus cabeçalhos de resposta em cache, garantindo que as futuras solicitações do CORS possam acessar o conteúdo em cache. Qualquer alteração de configuração do CORS no AEM Publish **deve** ser seguida por uma invalidação dos recursos em cache afetados. As práticas recomendadas determinam as implantações de código ou configuração nas quais o cache do dispatcher é removido, já que é difícil determinar qual conteúdo em cache pode ser afetado. |

### Permitir cabeçalhos de solicitação CORS

Para permitir que os [cabeçalhos de solicitação HTTP necessários sejam transmitidos para AEM para processamento](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders), eles devem ser permitidos na configuração `/clientheaders` do Dispatcher.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Armazenamento em cache de cabeçalhos de resposta CORS

Para permitir o armazenamento em cache e a veiculação de cabeçalhos CORS no conteúdo em cache, adicione a seguinte configuração [/cache /headers](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=pt-BR#caching-http-response-headers) ao arquivo AEM Publish `dispatcher.any`.

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

Lembre-se de **reiniciar o aplicativo do servidor Web** depois de fazer alterações no arquivo `dispatcher.any`.

Provavelmente, é necessário limpar totalmente o cache para garantir que os cabeçalhos sejam armazenados em cache de maneira adequada na próxima solicitação após uma atualização de configuração `/cache/headers`.

## Solução de problemas do CORS

O log está disponível em `com.adobe.granite.cors`:

* habilite `DEBUG` para ver detalhes sobre o motivo de uma solicitação [!DNL CORS] ter sido negada
* habilite `TRACE` para ver detalhes sobre todas as solicitações passando pelo manipulador CORS

### Dicas:

* Recriar manualmente solicitações XHR usando curl, mas certifique-se de copiar todos os cabeçalhos e detalhes, pois cada um pode fazer a diferença; alguns consoles do navegador permitem copiar o comando curl
* Verifique se a solicitação foi negada pelo manipulador CORS e não pela autenticação, filtro de token CSRF, filtros de dispatcher ou outras camadas de segurança
   * Se o manipulador CORS responder com 200, mas o cabeçalho `Access-Control-Allow-Origin` estiver ausente na resposta, verifique os logs de negações em [!DNL DEBUG] em `com.adobe.granite.cors`
* Se o cache do dispatcher de [!DNL CORS] solicitações estiver habilitado
   * Verifique se a configuração `/cache/headers` foi aplicada a `dispatcher.any` e se o servidor Web foi reiniciado com êxito
   * Verifique se o cache foi limpo corretamente após qualquer alteração de configuração de OSGi ou dispatcher.
* se necessário, verifique a presença de credenciais de autenticação na solicitação.

## Materiais de suporte

* [Fábrica de Configuração OSGi do AEM para Políticas de Compartilhamento de Recursos entre Origens](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Compartilhamento de Recursos entre Origens (W3C)](https://www.w3.org/TR/cors/)
* [Controle de Acesso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
