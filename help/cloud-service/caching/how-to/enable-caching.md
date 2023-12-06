---
title: Como ativar o armazenamento em cache do CDN
description: Saiba como habilitar o armazenamento em cache de respostas HTTP no CDN do AEM as a Cloud Service.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
source-git-commit: 43c021b051806380b3211f2d7357555622217b91
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 0%

---


# Como ativar o armazenamento em cache do CDN

Saiba como habilitar o armazenamento em cache de respostas HTTP no CDN do AEM as a Cloud Service. O armazenamento em cache de respostas é controlado pela `Cache-Control`, `Surrogate-Control`ou `Expires` Cabeçalhos de cache de resposta HTTP.

Esses cabeçalhos de cache normalmente são definidos em configurações de vhost do Dispatcher do AEM usando `mod_headers`, mas também pode ser definido no código Java™ personalizado em execução no próprio AEM Publish.

## Comportamento de cache padrão

Quando as configurações personalizadas NÃO estiverem presentes, os valores padrão serão usados. Na captura de tela a seguir, você pode ver o comportamento padrão de armazenamento em cache para AEM Publish e Author quando um [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) baseado `mynewsite` Projeto AEM implantado.

![Comportamento de cache padrão](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

Revise o [Publicação no AEM - Vida útil do cache padrão](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) e [Autor do AEM - Vida útil do cache padrão](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life) para obter mais informações.

Em resumo, o AEM as a Cloud Service armazena em cache a maioria dos tipos de conteúdo (HTML AEM AEM, JSON, JS, CSS e Assets) no Publish e alguns tipos de conteúdo (JS, CSS) no Author.

## Ativar armazenamento em cache

Para alterar o comportamento padrão de armazenamento em cache, você pode atualizar os cabeçalhos de cache de duas maneiras.

1. **Configuração do vhost do Dispatcher:** Disponível somente para publicação no AEM.
1. **Código Java™ personalizado:** Disponível para publicação no AEM e para Autor.

Vamos analisar cada uma dessas opções.

### Configuração do vhost do Dispatcher

Essa opção é a abordagem recomendada para ativar o armazenamento em cache, no entanto, só está disponível para publicação no AEM. Para atualizar os cabeçalhos de cache, use o `mod_headers` módulo e `<LocationMatch>` no arquivo vhost do Apache HTTP Server. A sintaxe geral é a seguinte:

    &quot;conf
    &lt;locationmatch url=&quot;&quot; url_regex=&quot;&quot;>
    # Remove o cabeçalho de resposta desse nome, se existir. Se houver vários cabeçalhos com o mesmo nome, todos serão removidos.
    Cabeçalho não definido Cache-Controle
    Cabeçalho não definido Surrogate-Control
    O cabeçalho não definido expira em
    
    # Instrui o navegador da Web e a CDN a armazenar em cache a resposta para o valor &quot;max-age&quot; (XXX) segundos. Os atributos &quot;stale-while-revalidate&quot; e &quot;stale-if-error&quot; controlam o tratamento do estado obsoleto na camada CDN.
    Conjunto de cabeçalhos Cache-Control &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;
    
    # Instrui o CDN a armazenar a resposta em cache para o valor &quot;max-age&quot; (XXX) segundos. Os atributos &quot;stale-while-revalidate&quot; e &quot;stale-if-error&quot; controlam o tratamento do estado obsoleto na camada CDN.
    Conjunto de cabeçalhos Surrogate-Control &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;
    
    # Instrui o navegador da Web e a CDN a armazenar a resposta em cache até a data e a hora especificadas.
    O conjunto de cabeçalhos expira em &quot;Sun, 31 de dezembro de 2023 23:59:59 GMT&quot;
    &lt;/locationmatch>
    &quot;

A seguir, é apresentado um resumo da finalidade de cada **cabeçalho** e aplicável **atributos** para o cabeçalho.

|                     | Navegador da Web | CDN | Descrição |
|---------------------|:-----------:|:---------:|:-----------:|
| Controle de cache | ✔ | ✔ | Esse cabeçalho controla a vida útil do navegador da Web e do cache CDN. |
| Surrogate-Control | ✘ | ✔ | Esse cabeçalho controla a vida útil do cache da CDN. |
| Expira em | ✔ | ✔ | Esse cabeçalho controla a vida útil do navegador da Web e do cache CDN. |


- **max-age**: Esse atributo controla o TTL ou o &quot;tempo de vida&quot; do conteúdo da resposta em segundos.
- **stale-while-revalidate**: Esse atributo controla o _estado obsoleto_ o tratamento do conteúdo da resposta na camada CDN quando a solicitação recebida se dá dentro do período especificado em segundos. A variável _estado obsoleto_ é o período de tempo após a expiração do TTL e antes da revalidação da resposta.
- **stale-if-error**: Esse atributo controla o _estado obsoleto_ tratamento do conteúdo da resposta na camada CDN quando o servidor de origem estiver indisponível e a solicitação recebida estiver dentro do período especificado em segundos.

Revise o [desatualização e revalidação](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) detalhes para obter mais informações.

#### Exemplo

Para aumentar a vida útil do navegador da Web e do cache da CDN do **tipo de conteúdo HTML** para _10 minutos_ sem o tratamento de estado obsoleto, siga estas etapas:

1. No projeto AEM, localize o arquivo vhsot desejado em `dispatcher/src/conf.d/available_vhosts` diretório.
1. Atualizar o vhost (por exemplo, `wknd.vhost`) da seguinte forma:

       &quot;conf
       &lt;locationmatch content=&quot;&quot;>*.(html)$&quot;>
       # Remove o cabeçalho de resposta, se presente
       Cabeçalho não definido Cache-Controle
       
       # Instrui o navegador da Web e a CDN a armazenar a resposta em cache para o valor max-age (600) segundos.
       Conjunto de cabeçalhos Cache-Control &quot;max-age=600&quot;
       &lt;/locationmatch>
       &quot;
   Os arquivos vhost em `dispatcher/src/conf.d/enabled_vhosts` diretório são **symlinks** aos arquivos em `dispatcher/src/conf.d/available_vhosts` diretório, portanto, crie symlinks se não estiver presente.
1. Implante as alterações do vhost no ambiente as a Cloud Service do AEM desejado usando o [Cloud Manager - Pipeline de configuração no nível da Web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) ou [Comandos RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

No entanto, para ter valores diferentes para a vida útil do navegador da Web e do cache de CDN, você pode usar o `Surrogate-Control` no exemplo acima. Da mesma forma que para expirar o cache em uma data e hora específicas, você pode usar o `Expires` cabeçalho. Além disso, usando o `stale-while-revalidate` e `stale-if-error` atributos, é possível controlar o tratamento de estado obsoleto do conteúdo da resposta. O projeto AEM WKND tem um [tratamento de estado obsoleto de referência](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) Configuração do cache do CDN.

Da mesma forma, também é possível atualizar os cabeçalhos de cache para outros tipos de conteúdo (JSON, JS, CSS e Assets).

### Código Java™ personalizado

Essa opção está disponível para publicação no AEM e para Autor. No entanto, não é recomendável ativar o armazenamento em cache no AEM Author e manter o comportamento padrão do armazenamento em cache.

Para atualizar os cabeçalhos de cache, use o `HttpServletResponse` no código Java™ personalizado (servlet Sling, filtro de servlet Sling). A sintaxe geral é a seguinte:

    &quot;java
    // Instrui o navegador da Web e o CDN a armazenar a resposta em cache para o valor &quot;max-age&quot; (XXX) segundos. Os atributos &quot;stale-while-revalidate&quot; e &quot;stale-if-error&quot; controlam o tratamento do estado obsoleto na camada CDN.
    response.setHeader(&quot;Cache-Control&quot;, &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;);
    
    // Instrui o CDN a armazenar a resposta em cache para o valor &quot;max-age&quot; (XXX) segundos. Os atributos &quot;stale-while-revalidate&quot; e &quot;stale-if-error&quot; controlam o tratamento do estado obsoleto na camada CDN.
    response.setHeader(&quot;Surrogate-Control&quot;, &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;);
    
    // Instrui o navegador da Web e a CDN a armazenar a resposta em cache até a data e a hora especificadas.
    response.setHeader(&quot;Expira&quot;, &quot;Sun, 31 de dezembro de 2023 23:59:GMT&quot;);
    &quot;
