---
title: Armazenamento em cache do serviço de publicação do AEM
description: Visão geral do armazenamento em cache do serviço de publicação do AEM as a Cloud Service.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
source-git-commit: 6cbd8f3c49d44e75337715c35c198008da8ae7b9
workflow-type: tm+mt
source-wordcount: '1320'
ht-degree: 2%

---


# AEM Publish

O serviço de Publicação do AEM tem duas camadas principais de armazenamento em cache, o CDN as a Cloud Service do AEM e o Dispatcher do AEM. Opcionalmente, uma CDN gerenciada pelo cliente pode ser colocada na frente da CDN as a Cloud Service do AEM. A CDN as a Cloud Service do AEM fornece entrega de conteúdo de ponta, garantindo que as experiências sejam entregues com baixa latência a usuários em todo o mundo. O AEM Dispatcher fornece armazenamento em cache diretamente na frente do AEM Publish e é usado para atenuar a carga desnecessária no AEM Publish.

![Diagrama de visão geral do armazenamento em cache de publicação do AEM](./assets/publish/publish-all.png){align="center"}

## CDN

O armazenamento em cache do AEM as a Cloud Service é controlado por cabeçalhos de cache de resposta HTTP e tem como objetivo armazenar em cache o conteúdo para otimizar um equilíbrio entre a atualização e o desempenho. O CDN fica entre o usuário final e o Dispatcher do AEM e é usado para armazenar em cache o conteúdo o mais próximo possível do usuário final, garantindo uma experiência com desempenho.

![CDN de publicação do AEM](./assets/publish/publish-cdn.png){align="center"}

Configurar como o conteúdo do CDN armazena em cache é limitado à configuração de cabeçalhos de cache em respostas HTTP. Esses cabeçalhos de cache normalmente são definidos em configurações de vhost do Dispatcher do AEM usando `mod_headers`, mas também pode ser definido no código Java™ personalizado em execução no próprio AEM Publish.

### Quando as solicitações/respostas HTTP são armazenadas em cache?

O AEM as a Cloud Service CDN armazena em cache somente respostas HTTP e todos os critérios a seguir devem ser atendidos:

+ O status da solicitação HTTP é `2xx` ou `3xx`
+ O método de solicitação HTTP é `GET` ou `HEAD`
+ Pelo menos um dos seguintes cabeçalhos de resposta HTTP está presente: `Cache-Control`, `Surrogate-Control`ou  `Expires`
+ A resposta HTTP pode ser qualquer tipo de conteúdo, incluindo HTML, JSON, CSS, JS e arquivos binários.

Por padrão, as respostas HTTP não são armazenadas em cache pelo [Dispatcher AEM](#aem-dispatcher) remova automaticamente todos os cabeçalhos de cache de resposta HTTP para evitar o armazenamento em cache na CDN. Esse comportamento pode ser cuidadosamente substituído usando `mod_headers` com o `Header always set ...` diretiva quando necessário.

### O que é armazenado em cache?

O CDN as a Cloud Service do AEM armazena em cache o seguinte:

+ Corpo da resposta HTTP
+ Cabeçalhos de resposta HTTP

Normalmente, uma solicitação/resposta HTTP para um único URL é armazenada em cache como um único objeto. No entanto, a CDN pode lidar com o armazenamento em cache de vários objetos para um único URL, quando a variável `Vary` cabeçalho estiver definido na resposta HTTP. Evitar especificar `Vary` nos cabeçalhos cujos valores não têm um conjunto de valores rigorosamente controlado, pois isso pode resultar em muitos erros de cache, reduzindo a taxa de ocorrência do cache. Para oferecer suporte ao armazenamento em cache de solicitações variáveis no AEM Dispatcher, [revisar a documentação do armazenamento em cache de variantes](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/variant-caching.html).

### Vida útil do cache{#cdn-cache-life}

O AEM Publish CDN é baseado em TTL (time-to-live), o que significa que a vida útil do cache é determinada pelo `Cache-Control`, `Surrogate-Control`ou `Expires` Cabeçalhos de resposta HTTP. Se os cabeçalhos de cache de resposta HTTP não estiverem definidos pelo projeto e a variável [critérios de elegibilidade](#when-are-http-requestsresponses-cached) forem atendidas, o Adobe definirá uma vida útil de cache padrão de 10 minutos (600 segundos).

Veja como os cabeçalhos de cache influenciam a vida útil do cache CDN:

+ [`Cache-Control`](https://developer.fastly.com/reference/http/http-headers/Cache-Control/) O cabeçalho de resposta HTTP instrui o navegador da Web e o CDN sobre por quanto tempo armazenar a resposta em cache. O valor é em segundos. Por exemplo, `Cache-Control: max-age=3600` instrui o navegador da web a armazenar a resposta em cache por uma hora. Esse valor é ignorado pela CDN se `Surrogate-Control` O cabeçalho de resposta HTTP também está presente.
+ [`Surrogate-Control`](https://developer.fastly.com/reference/http/http-headers/Surrogate-Control/) O cabeçalho de resposta HTTP instrui o CDN do AEM sobre por quanto tempo armazenar a resposta em cache. O valor é em segundos. Por exemplo, `Surrogate-Control: max-age=3600` instrui o CDN a armazenar a resposta em cache por uma hora.
+ [`Expires`](https://developer.fastly.com/reference/http/http-headers/Expires/) O cabeçalho de resposta HTTP instrui o CDN do AEM (e o navegador da Web) por quanto tempo a resposta em cache é válida. O valor é uma data. Por exemplo, `Expires: Sat, 16 Sept 2023 09:00:00 EST` instrui o navegador da web a armazenar a resposta em cache até a data e a hora especificadas.

Uso `Cache-Control` para controlar a vida útil do cache quando ela é a mesma para o navegador e o CDN. Uso `Surrogate-Control` quando o navegador da web deve armazenar a resposta em cache por uma duração diferente da CDN.

#### Vida útil do cache padrão

Se uma resposta HTTP se qualificar para cache do Dispatcher do AEM [por qualificadores acima](#when-are-http-requestsresponses-cached), a seguir estão os valores padrão, a menos que a configuração personalizada esteja presente.

| Tipo de conteúdo | Vida útil do cache padrão da CDN |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | 5 minutos |
| [Ativos (imagens, vídeos, documentos e assim por diante)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | 10 minutos |
| [Consultas persistentes (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 2 horas |
| [Bibliotecas de clientes (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 dias |
| [Outro](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Não armazenado em cache |

### Como personalizar regras de cache

[Configuração de como o CDN armazena conteúdo em cache](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#disp) O está limitado à configuração de cabeçalhos de cache em respostas HTTP. Normalmente, esses cabeçalhos de cache são definidos no AEM Dispatcher `vhost` configurações usando `mod_headers`, mas também pode ser definido no código Java™ personalizado em execução no próprio AEM Publish.

## AEM Dispatcher

![Dispatcher AEM de publicação do AEM no](./assets/publish/publish-dispatcher.png){align="center"}

### Quando as solicitações/respostas HTTP são armazenadas em cache?

As respostas HTTP para solicitações HTTP correspondentes são armazenadas em cache quando todos os critérios a seguir são atendidos:

+ O método de solicitação HTTP é `GET` ou `HEAD`
   + `HEAD` As solicitações HTTP armazenam em cache somente os cabeçalhos de resposta HTTP. Eles não têm corpos de resposta.
+ O status da resposta HTTP é `200`
+ A resposta HTTP NÃO é para um arquivo binário.
+ O caminho do URL da solicitação HTTP termina com uma extensão, por exemplo: `.html`, `.json`, `.css`, `.js`, etc.
+ As solicitações HTTP não contêm autorização e não são autenticadas pelo AEM.
   + No entanto, o armazenamento em cache de solicitações autenticadas [pode ser ativado globalmente](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-when-authentication-is-used) ou seletivamente via [armazenamento em cache sensível a permissões](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/permissions-cache.html?lang=pt-BR).
+ A solicitação HTTP não contém parâmetros de consulta.
   + No entanto, a configuração [Parâmetros de consulta ignorados](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#ignoring-url-parameters) permite que solicitações HTTP com os parâmetros de consulta ignorados sejam armazenadas em cache/fornecidas a partir do cache.
+ Caminho da solicitação HTTP [corresponde a uma regra para permitir o Dispatcher e não corresponde a uma regra para negar](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#specifying-the-documents-to-cache).
+ A resposta HTTP não tem nenhum dos seguintes cabeçalhos de resposta HTTP definidos pelo AEM Publish:

   + `no-cache`
   + `no-store`
   + `must-revalidate`

### O que é armazenado em cache?

O AEM Dispatcher armazena em cache o seguinte:

+ Corpo da resposta HTTP
+ Cabeçalhos de resposta HTTP especificados no cabeçalho do Dispatcher [configuração de cabeçalhos de cache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-http-response-headers). Consulte a configuração padrão fornecida com o [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L106-L113).
   + `Cache-Control`
   + `Content-Disposition`
   + `Content-Type`
   + `Expires`
   + `Last-Modified`
   + `X-Content-Type-Options`

### Vida útil do cache

O AEM Dispatcher armazena em cache as respostas HTTP usando as seguintes abordagens:

+ Até que a invalidação seja acionada por meio de mecanismos como a publicação ou despublicação do conteúdo.
+ TTL (vida útil) quando explicitamente [configurado na configuração do Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-time-based-cache-invalidation-enablettl). Consulte a configuração padrão no [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L122-L127) revisando o `enableTTL` configuração.

#### Vida útil do cache padrão

Se uma resposta HTTP se qualificar para cache do Dispatcher do AEM [por qualificadores acima](#when-are-http-requestsresponses-cached-1), a seguir estão os valores padrão, a menos que a configuração personalizada esteja presente.

| Tipo de conteúdo | Vida útil do cache padrão da CDN |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | Até a invalidação |
| [Ativos (imagens, vídeos, documentos e assim por diante)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | Nunca |
| [Consultas persistentes (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 1 minuto |
| [Bibliotecas de clientes (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 dias |
| [Outro](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Até a invalidação |

### Como personalizar regras de cache

O cache do Dispatcher do AEM pode ser configurado por meio do [Configuração do Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache) incluindo:

+ O que é armazenado em cache
+ Quais partes do cache são invalidadas ao publicar/desfazer a publicação
+ Quais parâmetros de consulta de solicitação HTTP são ignorados ao avaliar o cache
+ Quais cabeçalhos de resposta HTTP são armazenados em cache
+ Ativar ou desativar o armazenamento em cache TTL
+ ... e muito mais

Usar `mod_headers` para definir cabeçalhos de cache, `vhost` A configuração do não afetará o armazenamento em cache do Dispatcher (com base em TTL), pois eles são adicionados à resposta HTTP depois que o AEM Dispatcher processa a resposta. Para afetar o armazenamento em cache do Dispatcher por meio de cabeçalhos de resposta HTTP, é necessário o código Java™ personalizado em execução em AEM Publish, que define os cabeçalhos de resposta HTTP apropriados.
