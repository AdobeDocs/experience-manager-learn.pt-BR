---
title: Compreender o CORS (Cross-Origem Resource Sharing, compartilhamento de recursos em várias ) com AEM
description: O CORS (Cross-Origem Resource Sharing, Compartilhamento de recursos entre ) da Adobe Experience Manager facilita propriedades da Web não AEM para fazer chamadas do cliente para AEM, autenticadas e não autenticadas, buscar conteúdo ou interagir diretamente com AEM.
version: 6.3, 6,4, 6.5
sub-product: fundação, serviços de conteúdo, sites
feature: null
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
translation-type: tm+mt
source-git-commit: bc14783840a47fb79ddf1876aca1ef44729d097e
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 1%

---


# Compreender o compartilhamento de recursos entre Origens ([!DNL CORS])

O Compartilhamento de recursos entre Origens da Adobe Experience Manager ([!DNL CORS]) facilita as propriedades da Web que não são AEM para fazer chamadas do cliente para AEM, autenticadas e não autenticadas, buscar conteúdo ou interagir diretamente com AEM.

## Configuração OSGi da Política de Compartilhamento de Recursos entre Origens do Adobe Granite

As configurações de CORS são gerenciadas como fábricas de configuração OSGi em AEM, sendo cada política representada como uma instância da fábrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuração OSGi da Política de Compartilhamento de Recursos entre Origens do Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Seleção da política

Uma política é selecionada comparando a variável

* `Allowed Origin` com o cabeçalho da  `Origin` solicitação
* e `Allowed Paths` com o caminho da solicitação.

A primeira política que corresponde a esses valores será usada. Se nenhum for encontrado, qualquer solicitação [!DNL CORS] será negada.

Se nenhuma política estiver configurada, as solicitações [!DNL CORS] também não serão atendidas, pois o manipulador será desativado e, portanto, efetivamente negado - contanto que nenhum outro módulo do servidor responda a [!DNL CORS].

### Propriedades da política

#### [!UICONTROL Origens permitidas]

* `"alloworigin" <origin> | *`
* Lista de `origin` parâmetros que especificam URIs que podem acessar o recurso. Para solicitações sem credenciais, o servidor pode especificar * como um curinga, permitindo assim que qualquer origem acesse o recurso. *Não é recomendado usar  `Allow-Origin: *` na produção, pois permite que cada site estrangeiro (ou seja, um invasor) faça solicitações que sem CORS são estritamente proibidas pelos navegadores.*

#### [!UICONTROL Origens Permitidas (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lista de `regexp` expressões regulares especificando URIs que podem acessar o recurso. *Expressões regulares podem levar a correspondências não intencionais se não forem cuidadosamente criadas, permitindo que um invasor use um nome de domínio personalizado que também corresponderia à política.* Geralmente, é recomendado ter políticas separadas para cada nome de host de origem específico, usando  `alloworigin`, mesmo que isso signifique configuração repetida das outras propriedades de política. Diferentes origens tendem a ter ciclos de vida e requisitos diferentes, beneficiando assim de uma separação clara.

#### [!UICONTROL Caminhos permitidos]

* `"allowedpaths" <regexp>`
* Lista de `regexp` expressões regulares que especificam os caminhos de recursos aos quais a política se aplica.

#### [!UICONTROL Cabeçalhos expostos]

* `"exposedheaders" <header>`
* Lista de parâmetros de cabeçalho que indicam cabeçalhos de solicitação que os navegadores podem acessar.

#### [!UICONTROL Idade Máxima]

* `"maxage" <seconds>`
* Um parâmetro `seconds` que indica quanto tempo os resultados de uma solicitação de pré-voo podem ser armazenados em cache.

#### [!UICONTROL Cabeçalhos suportados]

* `"supportedheaders" <header>`
* Lista de `header` parâmetros que indicam quais cabeçalhos HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Métodos permitidos]

* `"supportedmethods"`
* Lista de parâmetros de método que indicam quais métodos HTTP podem ser usados ao fazer a solicitação real.

#### [!UICONTROL Suporta credenciais]

* `"supportscredentials" <boolean>`
* Um `boolean` indicando se a resposta à solicitação pode ou não ser exposta ao navegador. Quando usado como parte de uma resposta a uma solicitação de pré-voo, isso indica se a solicitação real pode ou não ser feita usando credenciais.

### Exemplo de configurações

O Site 1 é um cenário básico, acessível anonimamente, somente leitura, onde o conteúdo é consumido por meio de [!DNL GET] solicitações:

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

## Problemas de armazenamento em cache e configuração do Dispatcher {#dispatcher-caching-concerns-and-configuration}

A partir do Dispatcher 4.1.1+ os cabeçalhos de resposta podem ser armazenados em cache. Isso possibilita o cache de cabeçalhos [!DNL CORS] com os recursos [!DNL CORS] solicitados, desde que a solicitação seja anônima.

Geralmente, as mesmas considerações para o conteúdo em cache no Dispatcher podem ser aplicadas ao cache de cabeçalhos de resposta do CORS no dispatcher. A tabela a seguir define quando os cabeçalhos [!DNL CORS] (e, portanto, [!DNL CORS] solicitações) podem ser armazenados em cache.

| Acessível | Autor | Status de autenticação | Explicação |
|-----------|-------------|-----------------------|-------------|
| Não | AEM Publish | Autenticado | O cache do Dispatcher no AEM Author está limitado a ativos estáticos não criados. Isso torna difícil e impraticável armazenar a maioria dos recursos em cache no AEM Author, incluindo cabeçalhos de resposta HTTP. |
| Não | Publicar no AEM | Autenticado | Evite armazenar cabeçalhos CORS em cache em solicitações autenticadas. Isso se alinha à orientação comum de não armazenar solicitações autenticadas em cache, pois é difícil determinar como o status de autenticação/autorização do usuário solicitante afetará o recurso fornecido. |
| Sim | Publicar no AEM | Anônimo | As solicitações anônimas capazes de cache no dispatcher também podem ter seus cabeçalhos de resposta em cache em cache, garantindo que as solicitações futuras do CORS possam acessar o conteúdo em cache. Qualquer alteração na configuração do CORS no AEM Publish **deve** ser seguida por uma invalidação dos recursos em cache afetados. As práticas recomendadas ditam implantações de código ou configuração que o cache do dispatcher é removido, pois é difícil determinar que conteúdo em cache pode ser afetado. |

Para permitir o cache de cabeçalhos CORS, adicione a seguinte configuração a todos os arquivos despachantes.any do AEM Publish com suporte.

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

Lembre-se de **reiniciar o aplicativo do servidor Web** depois de fazer alterações no arquivo `dispatcher.any`.

Provavelmente, a limpeza total do cache será necessária para garantir que os cabeçalhos sejam adequadamente armazenados em cache na próxima solicitação após uma atualização de configuração `/clientheaders`.

## Solução de problemas do CORS

O registro está disponível em `com.adobe.granite.cors`:

* habilitar `DEBUG` para ver detalhes sobre por que uma solicitação [!DNL CORS] foi negada
* habilitar `TRACE` para ver detalhes sobre todas as solicitações que passam pelo manipulador CORS

### Dicas:

* Recrie manualmente as solicitações XHR usando a curva, mas certifique-se de copiar todos os cabeçalhos e detalhes, já que cada um pode fazer a diferença; alguns consoles de navegador permitem copiar o comando curl
* Verifique se a solicitação foi negada pelo manipulador CORS e não pela autenticação, filtro de token CSRF, filtros de despachante ou outras camadas de segurança
   * Se o manipulador do CORS responder com 200, mas o cabeçalho `Access-Control-Allow-Origin` estiver ausente na resposta, analise os registros em [!DNL DEBUG] em `com.adobe.granite.cors`
* Se o cache do dispatcher de solicitações [!DNL CORS] estiver ativado
   * Verifique se a configuração `/clientheaders` está aplicada a `dispatcher.any` e se o servidor Web foi reiniciado com êxito
   * Verifique se o cache foi limpo corretamente após qualquer OSGi ou dispatcher.quaisquer alterações de configuração.
* se necessário, verifique a presença de credenciais de autenticação na solicitação.

## Materiais de suporte

* [Fábrica de configuração AEM OSGi para Políticas de Compartilhamento de Recursos de Origem Cruzada](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Compartilhamento de recursos entre Origens (W3C)](https://www.w3.org/TR/cors/)
* [CONTROLE DE ACESSO HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
