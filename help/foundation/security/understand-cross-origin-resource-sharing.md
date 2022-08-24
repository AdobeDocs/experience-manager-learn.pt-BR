---
title: Entenda o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens) com a AEM
description: O CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos de várias origens) da Adobe Experience Manager facilita as propriedades da Web que não são AEM para fazer chamadas do lado do cliente para AEM, autenticadas e não autenticadas, buscar conteúdo ou interagir diretamente com AEM.
version: 6.4, 6.5
sub-product: foundation, content-services, sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: 41be8c934bba16857d503398b5c7e327acd8d20b
workflow-type: tm+mt
source-wordcount: '914'
ht-degree: 1%

---

# Compreender o compartilhamento de recursos entre origens ([!DNL CORS])

Compartilhamento de recursos entre origens da Adobe Experience Manager ([!DNL CORS]) facilita propriedades da Web que não são AEM para fazer chamadas do lado do cliente para AEM, autenticadas ou não, buscar conteúdo ou interagir diretamente com AEM.

## Configuração OSGi da Política de Compartilhamento de Recursos entre Origens do Adobe Granite

As configurações do CORS são gerenciadas como fábricas de configuração OSGi no AEM, com cada política sendo representada como uma instância da fábrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuração OSGi da Política de Compartilhamento de Recursos entre Origens do Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Seleção da política

Uma política é selecionada comparando a

* `Allowed Origin` com o `Origin` cabeçalho da solicitação
* e `Allowed Paths` com o caminho da solicitação.

A primeira política correspondente a esses valores será usada. Se nenhum for encontrado, qualquer [!DNL CORS] será negada.

Se nenhuma política estiver configurada, [!DNL CORS] as solicitações também não serão respondidas, pois o manipulador será desativado e, portanto, efetivamente negado - desde que nenhum outro módulo do servidor responda a [!DNL CORS].

### Propriedades da política

#### [!UICONTROL Origens permitidas]

* `"alloworigin" <origin> | *`
* Lista de `origin` parâmetros que especificam URIs que podem acessar o recurso. Para solicitações sem credenciais, o servidor pode especificar &#42; como um curinga, permitindo assim que qualquer origem acesse o recurso. *Não é recomendado usar `Allow-Origin: *` em produção , pois permite que cada site estrangeiro (ou seja, um invasor) faça solicitações que sem CORS sejam estritamente proibidas pelos navegadores.*

#### [!UICONTROL Origens Permitidas (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lista de `regexp` expressões regulares especificando URIs que podem acessar o recurso. *Expressões regulares podem levar a correspondências não intencionais se não forem criadas com cuidado, permitindo que um invasor use um nome de domínio personalizado que também corresponderia à política.* Geralmente, é recomendável ter políticas separadas para cada nome de host de origem específica, usando `alloworigin`, mesmo que isso signifique a configuração repetida das outras propriedades da política. Diferentes origens tendem a ter ciclos de vida e requisitos diferentes, beneficiando assim de uma separação clara.

#### [!UICONTROL Caminhos permitidos]

* `"allowedpaths" <regexp>`
* Lista de `regexp` expressões regulares especificando caminhos de recursos aos quais a política se aplica.

#### [!UICONTROL Cabeçalhos expostos]

* `"exposedheaders" <header>`
* Lista de parâmetros de cabeçalho que indicam cabeçalhos de solicitação que os navegadores podem acessar.

#### [!UICONTROL Idade máxima]

* `"maxage" <seconds>`
* A `seconds` parâmetro indicando por quanto tempo os resultados de uma solicitação de pré-voo podem ser armazenados em cache.

#### [!UICONTROL Cabeçalhos suportados]

* `"supportedheaders" <header>`
* Lista de `header` parâmetros que indicam quais cabeçalhos HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Métodos permitidos]

* `"supportedmethods"`
* Lista de parâmetros de método que indicam quais métodos HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Suporta Credenciais]

* `"supportscredentials" <boolean>`
* A `boolean` indicando se a resposta à solicitação pode ou não ser exposta ao navegador. Quando usado como parte de uma resposta a uma solicitação de pré-voo, isso indica se a solicitação real pode ou não ser feita usando credenciais.

### Exemplo de configurações

O Site 1 é um cenário básico, acessível anonimamente, somente leitura em que o conteúdo é consumido por meio de [!DNL GET] solicitações:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site1.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site1/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

O Site 2 é mais complexo e requer solicitações autorizadas e não seguras:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site2.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site2/.*,/libs/granite/csrf/token.json]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers,Authorization,CSRF-Token]"
    supportedmethods="[GET,HEAD,POST,DELETE,OPTIONS,PUT]"
    supportscredentials="{Boolean}true"
/>
```

## Problemas e configuração de armazenamento em cache do Dispatcher {#dispatcher-caching-concerns-and-configuration}

A partir do Dispatcher 4.1.1+, os cabeçalhos de resposta podem ser armazenados em cache. Isso permite armazenar em cache [!DNL CORS] cabeçalhos junto com o [!DNL CORS]-recursos solicitados, desde que a solicitação seja anônima.

Geralmente, as mesmas considerações para o armazenamento em cache de conteúdo no Dispatcher podem ser aplicadas ao armazenamento em cache de cabeçalhos de resposta do CORS no dispatcher. A tabela a seguir define quando [!DNL CORS] cabeçalhos (e, portanto, [!DNL CORS] solicitações) podem ser armazenadas em cache.

| Acessível | Autor | Status de autenticação | Explicação |
|-----------|-------------|-----------------------|-------------|
| Não | AEM Publish | Autenticado | O armazenamento em cache do Dispatcher no AEM Author é limitado a ativos estáticos não criados. Isso torna difícil e impraticável armazenar em cache a maioria dos recursos no AEM Author, incluindo cabeçalhos de resposta HTTP. |
| Não | Publicação do AEM | Autenticado | Evite armazenar cabeçalhos CORS em cache em solicitações autenticadas. Isso se alinha à orientação comum de não armazenar em cache solicitações autenticadas, pois é difícil determinar como o status de autenticação/autorização do usuário solicitante afetará o recurso fornecido. |
| Sim | Publicação do AEM | Anônimo | As solicitações anônimas capazes de armazenar em cache no dispatcher também podem ter seus cabeçalhos de resposta em cache, garantindo que futuras solicitações do CORS possam acessar o conteúdo em cache. Qualquer alteração na configuração do CORS na publicação do AEM **must** ser seguida por uma invalidação dos recursos em cache afetados. As práticas recomendadas ditam as implantações de código ou configuração em que o cache do dispatcher é limpo, pois é difícil determinar qual conteúdo em cache pode ser afetado. |

Para permitir o armazenamento em cache de cabeçalhos CORS, adicione a seguinte configuração a todos os arquivos AEM Publish dispatcher.any de suporte.

```
/cache { 
  ...
  /headers {
      "Origin"
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

Lembrar de **reinicie o aplicativo do servidor web** depois de fazer alterações no `dispatcher.any` arquivo.

É provável que a limpeza total do cache seja necessária para garantir que os cabeçalhos sejam armazenados em cache adequadamente na próxima solicitação após uma `/cache/headers` atualização de configuração.

## Solução de problemas do CORS

O registro está disponível em `com.adobe.granite.cors`:

* habilitar `DEBUG` para ver detalhes sobre por que uma [!DNL CORS] solicitação negada
* habilitar `TRACE` para ver detalhes sobre todas as solicitações que passam pelo manipulador do CORS

### Dicas:

* Recrie manualmente as solicitações XHR usando curl, mas certifique-se de copiar todos os cabeçalhos e detalhes, pois cada um pode fazer a diferença; alguns consoles de navegador permitem copiar o comando curl
* Verifique se a solicitação foi negada pelo manipulador do CORS e não pela autenticação, pelo filtro de token CSRF, pelos filtros de dispatcher ou por outras camadas de segurança
   * Se o manipulador de CORS responder com 200, mas `Access-Control-Allow-Origin` estiver ausente na resposta, revise os logs para negações em [!DNL DEBUG] em `com.adobe.granite.cors`
* Se o armazenamento em cache do dispatcher [!DNL CORS] as solicitações estão habilitadas
   * Verifique se a `/cache/headers` a configuração é aplicada a `dispatcher.any` e o servidor Web foi reiniciado com êxito
   * Verifique se o cache foi limpo corretamente após qualquer alteração de configuração de OSGi ou dispatcher.any.
* se necessário, verifique a presença de credenciais de autenticação na solicitação.

## Materiais de apoio

* [AEM Fábrica de configuração OSGi para Políticas de Compartilhamento de Recursos entre Origens](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Compartilhamento de recursos entre origens (W3C)](https://www.w3.org/TR/cors/)
* [Controle de acesso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
