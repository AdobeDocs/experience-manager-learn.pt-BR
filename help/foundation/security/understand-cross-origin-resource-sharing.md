---
title: Entenda o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens) com o AEM
description: O CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos de várias origens) do Adobe Experience Manager facilita as propriedades da Web que não são do AEM para fazer chamadas do lado do cliente para o AEM, autenticadas e não autenticadas, para buscar conteúdo ou interagir diretamente com o AEM.
version: 6.3, 6,4, 6.5
sub-product: fundação, serviços de conteúdo, sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 1%

---


# Compreender o compartilhamento de recursos entre origens ([!DNL CORS])

O Compartilhamento de recursos entre origens do Adobe Experience Manager ([!DNL CORS]) facilita as propriedades da Web que não são do AEM para fazer chamadas do lado do cliente para o AEM, autenticadas e não autenticadas, para buscar conteúdo ou interagir diretamente com o AEM.

## Configuração OSGi da Política de Compartilhamento de Recursos entre Origens do Adobe Granite

As configurações de CORS são gerenciadas como fábricas de configuração OSGi no AEM, com cada política sendo representada como uma instância da fábrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuração OSGi da Política de Compartilhamento de Recursos entre Origens do Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Seleção da política

Uma política é selecionada comparando a

* `Allowed Origin` com o cabeçalho da  `Origin` solicitação
* e `Allowed Paths` com o caminho da solicitação.

A primeira política correspondente a esses valores será usada. Se nenhuma for encontrada, qualquer solicitação [!DNL CORS] será negada.

Se nenhuma política estiver configurada, as solicitações [!DNL CORS] também não serão respondidas, pois o manipulador será desativado e, portanto, efetivamente negado - desde que nenhum outro módulo do servidor responda a [!DNL CORS].

### Propriedades da política

#### [!UICONTROL Origens permitidas]

* `"alloworigin" <origin> | *`
* Lista de parâmetros `origin` especificando URIs que podem acessar o recurso. Para solicitações sem credenciais, o servidor pode especificar * como curinga, permitindo que qualquer origem acesse o recurso. *Não é recomendado usar  `Allow-Origin: *` em produção, pois permite que cada site estrangeiro (ou seja, um invasor) faça solicitações que sem CORS sejam estritamente proibidas pelos navegadores.*

#### [!UICONTROL Origens Permitidas (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lista de `regexp` expressões regulares especificando URIs que podem acessar o recurso. *Expressões regulares podem levar a correspondências não intencionais se não forem criadas com cuidado, permitindo que um invasor use um nome de domínio personalizado que também corresponderia à política.* Geralmente, é recomendável ter políticas separadas para cada nome de host de origem específica, usando  `alloworigin`, mesmo que isso signifique a configuração repetida das outras propriedades da política. Diferentes origens tendem a ter ciclos de vida e requisitos diferentes, beneficiando assim de uma separação clara.

#### [!UICONTROL Caminhos permitidos]

* `"allowedpaths" <regexp>`
* Lista de expressões regulares `regexp` especificando caminhos de recursos aos quais a política se aplica.

#### [!UICONTROL Cabeçalhos expostos]

* `"exposedheaders" <header>`
* Lista de parâmetros de cabeçalho que indicam cabeçalhos de solicitação que os navegadores podem acessar.

#### [!UICONTROL Idade máxima]

* `"maxage" <seconds>`
* Um parâmetro `seconds` indicando por quanto tempo os resultados de uma solicitação de pré-voo podem ser armazenados em cache.

#### [!UICONTROL Cabeçalhos suportados]

* `"supportedheaders" <header>`
* Lista de parâmetros `header` indicando quais cabeçalhos HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Métodos permitidos]

* `"supportedmethods"`
* Lista de parâmetros de método que indicam quais métodos HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Suporta Credenciais]

* `"supportscredentials" <boolean>`
* Um `boolean` indicando se a resposta à solicitação pode ou não ser exposta ao navegador. Quando usado como parte de uma resposta a uma solicitação de pré-voo, isso indica se a solicitação real pode ou não ser feita usando credenciais.

### Exemplo de configurações

O Site 1 é um cenário básico, acessível anonimamente, somente leitura, onde o conteúdo é consumido por meio de solicitações [!DNL GET]:

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

A partir do Dispatcher 4.1.1+, os cabeçalhos de resposta podem ser armazenados em cache. Isso possibilita armazenar em cache [!DNL CORS] cabeçalhos juntamente com os recursos [!DNL CORS] solicitados, desde que a solicitação seja anônima.

Geralmente, as mesmas considerações para o armazenamento em cache de conteúdo no Dispatcher podem ser aplicadas ao armazenamento em cache de cabeçalhos de resposta do CORS no dispatcher. A tabela a seguir define quando os cabeçalhos [!DNL CORS] (e, portanto, as solicitações [!DNL CORS]) podem ser armazenados em cache.

| Acessível | Autor | Status de autenticação | Explicação |
|-----------|-------------|-----------------------|-------------|
| Não | AEM Publish | Autenticado | O armazenamento em cache do Dispatcher no AEM Author é limitado a ativos estáticos não criados. Isso torna difícil e impraticável armazenar em cache a maioria dos recursos no AEM Author, incluindo cabeçalhos de resposta HTTP. |
| Não | Publicação do AEM | Autenticado | Evite armazenar cabeçalhos CORS em cache em solicitações autenticadas. Isso se alinha à orientação comum de não armazenar em cache solicitações autenticadas, pois é difícil determinar como o status de autenticação/autorização do usuário solicitante afetará o recurso fornecido. |
| Sim | Publicação do AEM | Anônimo | As solicitações anônimas capazes de armazenar em cache no dispatcher também podem ter seus cabeçalhos de resposta em cache, garantindo que futuras solicitações do CORS possam acessar o conteúdo em cache. Qualquer alteração na configuração do CORS em AEM Publish **deve** ser seguida por uma invalidação dos recursos em cache afetados. As práticas recomendadas ditam as implantações de código ou configuração em que o cache do dispatcher é limpo, pois é difícil determinar qual conteúdo em cache pode ser afetado. |

Para permitir o armazenamento em cache de cabeçalhos CORS, adicione a seguinte configuração a todos os arquivos AEM Publish dispatcher.any de suporte.

```
/cache { 
  ...
  /clientheaders {
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

Lembre-se de **reiniciar o aplicativo do servidor da Web** depois de fazer alterações no arquivo `dispatcher.any`.

É provável que a limpeza total do cache seja necessária para garantir que os cabeçalhos sejam armazenados em cache adequadamente na próxima solicitação após uma atualização de configuração `/clientheaders`.

## Solução de problemas do CORS

O registro está disponível em `com.adobe.granite.cors`:

* habilite `DEBUG` para ver detalhes sobre por que uma solicitação [!DNL CORS] foi negada
* habilite `TRACE` para ver detalhes sobre todas as solicitações que passam pelo manipulador do CORS

### Dicas:

* Recrie manualmente as solicitações XHR usando curl, mas certifique-se de copiar todos os cabeçalhos e detalhes, pois cada um pode fazer a diferença; alguns consoles de navegador permitem copiar o comando curl
* Verifique se a solicitação foi negada pelo manipulador do CORS e não pela autenticação, pelo filtro de token CSRF, pelos filtros de dispatcher ou por outras camadas de segurança
   * Se o manipulador do CORS responder com 200, mas o cabeçalho `Access-Control-Allow-Origin` estiver ausente na resposta, revise os logs para negações em [!DNL DEBUG] em `com.adobe.granite.cors`
* Se o armazenamento em cache do dispatcher de solicitações [!DNL CORS] estiver ativado
   * Verifique se a configuração `/clientheaders` está aplicada a `dispatcher.any` e se o servidor da Web foi reiniciado com êxito
   * Verifique se o cache foi limpo corretamente após qualquer alteração de configuração de OSGi ou dispatcher.any.
* se necessário, verifique a presença de credenciais de autenticação na solicitação.

## Materiais de apoio

* [Fábrica de configuração do AEM OSGi para políticas de compartilhamento de recursos entre origens](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Compartilhamento de recursos entre origens (W3C)](https://www.w3.org/TR/cors/)
* [Controle de acesso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
